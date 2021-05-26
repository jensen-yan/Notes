### 奔跑吧linux准备

#### linux版本

1. 红帽

   1. 稳定, 适合企业, 服务器
   2. 但不适合个人, 软件少, 要收费

2. centos: 克隆红帽

   1. 稳定, 免费
   2. 但不适合个人, 软件少

3. ubuntu: 

   1. 基于debian, 有apt打包, dde
   2. 友好界面设计 , 有强大社区, 有中文
   3. 软件包丰富, 适合开发者使用

4. 推荐ubuntu 16.04: 有默认

   

开发板: 适合开发者学习某个芯片, 快速开发产品.

推荐: 树莓派开发板 arm A53 cpu, 有蓝牙, wifi, 200元

或者使用qumu: 

1. 支持多种cpu架构: arm, x86
2. 支持单步调试内核
3. 支持丰富外设
4. gdb+eclipse 实现图形化单步调试



#### 能力

linux运维人员能力

1. 入门级: 会安装
2. 初级: 
   1. 读懂内核log信息
   2. top, vmstat查看系统信息
   3. 读懂meminfo 
   4. 使用内核参数调节
   5. 例如: 红帽官方文档: performace guide
3. 中级:
   1. 使用debug工具: kdump, systemtap, 内存泄露查找
   2. 读懂内核出错信息
   3. 对子系统代码熟悉了解, 内存管理, 进程管理
4. 高级:
   1. 读懂大部分核心代码, 了解不足
   2. 在社区打补丁



#### qemu + gdb

注意交叉编译器要5.5版本, 我放在software文件中了, 在编译时候要指定

make -j8 大概只用2分钟, 还挺快

```bash
sh run.sh arm32

cat /proc/cpuinfo  查看cpu信息
cat /proc/meminfo 查看mem信息

poweroff  # 关机

sh run.sh arm32 debug
# 另一个终端
arm-linux-gnueabihf-gdb --tui vmlinux
target remote:1234
b do_fork
c
# 使用eclipse
# 点击debug, 另一个终端
arm-linux-gnueabihf-gdb vmlinux
$shell file vmlinux
b do_fork
c
```



#### 硬件内存管理

1. 0-3G 用户空间, 3-4G 内核空间
2. 用户空间 malloc/mmap/mlock/madvise/mremap , malloc基于brk系统调用, sys_mmap. 
3. vma管理, vm_area_struct管理, 创建管理, 返回vma地址(虚拟)
4. 缺页中断
5. 匿名页面(没关联任何文件的页面 mmap), page cache(有关联, 但在cache中), slab, 页面回收
6. 页面分配器(伙伴系统)
7. 页表管理(内核, 进程页表)

![image-20210512175408130](/home/yanyue/.config/Typora/typora-user-images/image-20210512175408130.png)

1. huge page: 用于服务器, 对于大页分配, 减少缺页异常
2. 页迁移
3. 用task_struct来管理进程, mm-> mm_struct -> mmap, pgd



#### 阅读

1. source insight
   1. Windows 最强大代码索引, 查找编辑工具
   2. 用samba 共享目录到Windows
2. vim: 加上插件ctag, cscope
3. grep
4. find

注意使用-O0 不优化, 防止代码乱跳



教材: linux0.11代码剖析



### 操作系统基础 李治军

#### os启动

和保护模式对应, 实模式寻址是cs:ip   = cs << 4 + ip 段地址+偏移

##### bootsect.s

第一段代码

1. 上电时候: 寻址0xffff0

2. 检查RAM, 键盘, 显示器, 软盘磁盘
3. 把磁盘0此道0扇区读到0x7c00处(512byte, 引导扇区)
4. 设置cs=0x07c0, ip = 0x0000

```assembly
start:
	mov ds = 0x
```

256个字=512byte(1个word=16位=0x1234)

把0x7c00的256个字移动到0x9000, 腾出空间

jmpi go, 0x9000 	# 跳转到90000 + go偏移位置(段间跳转) 

从0x7c00 扇区调到0x90000扇区执行

int 0x13 读扇区BIOS中断

从磁盘第二个扇区开始读, 读四个扇区setup, 读取到内存0x90200 位置

bootsect.s作用: 

1. 读取setup的代码
2. 打印logo
3. 读入后面的os代码system
4. jump setup 



##### setup.s

获取计算机基础信息: 内存大小, 显卡参数等. 方便os使用数据结构来管理这些资源

1. 扩展内存大小
2. 把0x90000之后的所有os代码, 移动到0地址(可能覆盖0x7c00, 所以之前提前移动出去了)
3. jmpi 0,8  (段内跳转指令)ip = 0, cs = 8, addr = 0x80 system模块, 跳过去会死机, 实际上是跳转到0地址, 这里寻址模式变化了, 进入保护模式
   1. 实模式: cs << 4 + ip, 只能寻址1M地址空间, 太小
   2. 保护模式:寻址4G, 让cr0->PE = 1即可, 使用gdt(global descriptor table), 类似页表? cs寻址到gdt对应的页号 + ip => addr, 只要提前初始化gdt, 就可以用jmpi 0,8 找到gdt第8byte内容=0, jmp 0x0
   3. <img src="/home/yanyue/.config/Typora/typora-user-images/image-20210513090851085.png" alt="image-20210513090851085" style="zoom:80%;" />
      1. 保护模式下: 中断 int n 也是查询IDT(interrupt descriptor table)表的第n个表项来确定中断内容



![image-20210513091240376](/home/yanyue/.config/Typora/typora-user-images/image-20210513091240376.png)

跳到0地址后, 从system模块0地址开始执行(由一堆文件编译生成, 使用Makefile, 第一部分是head.s )

保护模式下: movl $0x10, %eax, (eax = 10, 源在前, 目标在后, AT&T语法, 和riscv这些不同)

##### head.s

(一直在保护模式下):

1. 初始化idt, gdt表
2. 设置页表
3. 跳到main.c, 先把参数从后往前压栈(栈从上往下生长), 然后压入返回地址, 子函数结束时ret指令让返回地址出栈, jump 过去
4. 这里main函数的返回地址L6是jmp L6无限循环, 死机, main不应该返回



##### main.c

1. 初始化mem(使用mem_map数组标志哪些页没使用, 前面页用了, end_mem这个参数在setup.s 0x90002地址设置并传递过来)
2. 初始化tty(键盘)等
3. fork主线程



#### 实验1

环境: bochs + gcc + sublime + linux0.11, hit-oslab集成环境

目标: 修改linux0.11源码, gcc编译, 在Bochs 虚拟环境运行调试代码

Bochs: x86模拟器

tar -zxvf  zip  -C dir # -C 输出到dir目录中

1. image 编译后的目标文件, 会挂载到软盘中
2. run: 在虚拟软驱A中挂在linun-0.11/image, 硬盘挂载 hdc-0.11.img, 存放minix文件系统镜像, 可以通过mount访问内容
3. hdc-0.11.img包含shlel, gcc 1.4 as86 ld86 , linux0.11代码, 编译可以覆盖



ls -lh 显示文件大小, 单位合适

file a.out 显示文件信息

nm a.out 显示文件的各个符号

strip a.out 给文件脱掉外衣, 删除一些符号信息和调试信息, 文件变小, 还可以对动态库.so文件, 减小空间. strip之前用来调试, strip之后用来发布.

```Makefile
# linux
Image: boot/bootsect boot/setup tools/system tools/build
	cp -f tools/system system.tmp	# -f强制赋值
	strip system.tmp	# 删除调试信息
	objcopy -O binary -R .note -R .comment system.tmp tools/kernel	# -O 格式为二进制, -R 删除.note, .comment段信息, 把system.tmp 拷贝到kernel中(改名了)
	tools/build boot/bootsect boot/setup tools/kernel $(ROOT_DEV) > Image 	# 使用build工具, 把后三者集成到Image中
	rm system.tmp
	rm tools/kernel -f
	sync	# 把缓存内容写到磁盘中
```



两种调试

1. ./gdb-asm 汇编调试, help查看方法
2. ./gdb-c 另一个终端  ./rungdb 可以使用gdb调试



文件交换

1. sudo ./mount-hdc 让0.11内核文件系统挂载到oslab/hdc中
2. 
3. 可以在/hdc中新建.c文件, 然后进入0.11系统后, 就有这个文件了, 防止在0.11中编辑文件, 比较麻烦
4. 0.11产生的文件也可以在/hdc中看到
5. sudo umount hdc 卸载, /hdc内容为空
6. 注意0.11运行时候, 不要挂载! 关闭bochs之前, 用sync确保保持到磁盘后再关闭!



#### 实验2

目标

1. 改bootsect.s 改输出内容
2. 改setup.s 



使用hexdump -C bootsect 可以查看二进制代码

dd bs=1 if=bootsect of=Image skip=32 把bootset文件去掉前32byte, 拷贝到Image中

