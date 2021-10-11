#### 运行方法

```shell
make qemu
ctrl+a  x #关闭qemu

make qemu-gdb
#另一个终端
riscv64-unknown-linux-gnu-gdb
file kernel/kernel
target remote:26000
b main
r

# 调试user文件方法
file user/ls.c
b ls.c:main

# 使用ctrl+x可以查看当前进程 在console.c中有定义
```



太奇怪了吧, sh.c 在150行就能打断点, 其他地方又不行...



#### 进程切换相关

1. 用户态的trap
   1. uservec -> usertrap(trap.c) -> usertrapret -> userret(trampoline.S)
   2. 比从内核态切换更难, 切换页表时候, 用户页表还没有map kernel页
   3. uservec 改satp来执行内核页表, 还有指向相同地址
   4. xv6让trapoline页映射到内核, 每个用户空间, 的同一个虚地址上



_entry -> start() -> main()->scheduler() -> swtch() -> forkret()->usertrapret()->userret()

userret会切换页表到用户进程, 是谁呢?

usertrap -> syscall(exec) -> exec()

exec 检查elf头, 分配用户页, 用户栈页, 加载init程序到内存中, 初始化用户栈(传argv入栈)

之后进入init.c 用户进程, open(console), 启动sh

其中start最后执行mret, pc = mepc的值



可以在sh.asm中找到write在0xdea处

gdb: b *0xdea  ; x/3i $pc;   si

ecall: 做三件事

1. 把模式从用户态变成主管态
2. sepc = pc
3. jump to stvec (蹦床页, 0x3fffffff000处, trampoline.S uservec处)

eret:

1. 根据sstatus的内容开中断等
2. pc = sepc

uservec:

1. 保存用户进程的32个寄存器
   1. 把trapframe映射到每个用户页表中, 在0x3ffffffe000处, 蹦床页下面
   2. sscratch, 指向这个地址
2. 切换到内核页表
3. 为内核C代码设置堆栈, sp, tp, t0



#### 函数调用

1. 栈向下生长, sp-=16
2. sp指向栈顶, 最下面; fp指向当前帧开头
3. a0 - a7 以及栈上传参, a0, a1是返回结果
4. caller-saved: 调用者要保存的reg, 如果reg值有意义, 在调用前先保存, 防止函数调用结束后污染reg. 函数就可以随便用这些reg
   1. 包含ra, t0, a0-a7
5. callee-saved: 被调用者要保持的reg, 函数调用返回后这个reg不变, 需要把这些提前放在栈中, 之后取出来, 然后函数就可以对这些reg为所欲为了
   1. 包含sp, fp, s2-s11 

![image-20210602092927902](/home/yanyue/.config/Typora/typora-user-images/image-20210602092927902.png)

对叶子函数, 不需要存ra, 直接返回即可. 非叶子函数, 需要存ra, 以免调用新函数把ra覆盖了, 需要存在栈中

```
addi sp, sp, -16
sd 	 ra, 0(sp)
...
ld 	 ra, 0(sp)
addi sp, sp, 16

      +->          .
      |   +-----------------+   |
      |   | return address  |   |
      |   |   previous fp ------+
      |   | saved registers |
      |   | local variables |
      |   |       ...       | <-+
      |   +-----------------+   |
      |   | return address  |   |
      +------ previous fp   |   |
          | saved registers |   |
          | local variables |   |
      +-> |       ...       |   |
      |   +-----------------+   |
      |   | return address  |   |
      |   |   previous fp ------+
      |   | saved registers |
      |   | local variables |
      |   |       ...       | <-+
      |   +-----------------+   |
      |   | return address  |   |
      +------ previous fp   |   |
          | saved registers |   |
          | local variables |   |
  $fp --> |       ...       |   |
          +-----------------+   |
          | return address  |   |
          |   previous fp ------+
          | saved registers |
  $sp --> | local variables |
          +-----------------+
```







call label:

1. ra = pc + 4, ra存返回地址
2. pc = label (使用的pc相对跳转or直接跳转)

ret:

​	jump ra( pc = ra)



正常运行程序时候, 到底有几个正在运行的进程?

proc[0] = init  始终sleep, 正常

proc[1] = sh : 感觉应该经常running才对啊, 但似乎是sleep的

proc[2] = alarmtest: 一直running, 所以每次sched一直调度自己, 没变 



#### trampoline

区别trapframe:

trampoline蹦床页是可执行页, 只是用来存放uservec汇编代码, 然后内核态和用户进程都映射高地址到同一个物理地址上, 这样但用户陷入内核态, 能执行同一段程序. 注意没有U模式, 只能监管态执行

trapframe是存放一些进程切换上下文寄存器的, 可读可写. 只是存一些数据



#### lab5: lazy

一般会延迟分配用户堆空间. sbrk不分配物理内存, 只是记住哪些用户地址, 并在页表中标记成invalid, 当进程第一次使用这个地址, 产生page fault. 去分配物理内存并清零, 写入valid来中断处理. 本实验就是实现这个特性.

16个异常有3个和分页相关, 分别是指令 / load / store  page fault. 

是同步异常, 类似syscall, 执行到那里去处理.

1. stval : 存储发送错误虚地址
2. scause: 错误类型, 上面3种
3. 指令: 用户态: tf->epc. U/K: 在usertrap/kerneltrap中

usertrap(): unexpected scause 0x000000000000000f pid=3
            sepc=0x00000000000012ac stval=0x0000000000004008
panic: uvmunmap: not mapped

表示cause=f=15=store page fault.

sepc = 0x12ac 保存的用户发送例外的pc, 一条sw指令

stval = 0x4008 这个地址（用户态第4页没分配发生缺页）



copy-on-write fork: fork首先把父子虚存映射到同一个物理页(只读), 但需要sw时, 发送page fault, copy一个可读写页给子进程空间, copy一个可读写页给父进程空间, 然后回到例外发生地方 

出现printf原因: sbrk没有申请空间, 写入页表映射. 而进程释放时: 

wait -> freeproc -> proc_freepagetable -> ubmfree -> uvmunmap (panic)

只是sz增加, 但是释放要按照sz来释放大小, 释放的pte的V为0, panic



bug1: panic: uvmunmap: walk, 在freeproc时发现pte=0, 说明没有映射pte

mappages: 默认V=1, 其他位任意. 发现页表有很多项, 但只有V=1

lazy alloc 成功!



bug2: panic: uvmcopy: pte should exist

应该是fork时的问题



bug3: stacktest : panic: remap

子进程会尝试访问保护页, 然后出错.

要处理user stack 下面的无效缺页异常



bug4: test sbrkarg: sbrkarg: write sbrk failed

进程传递sbrk的有效地址给syscall, 但是地址的内存还没分配

去查看addr 是否已经分配吗?
