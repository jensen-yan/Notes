### lab6: copy on write

页表能有一些技巧, 内核能拦截内存访问(让pte invalid 或者只读), 产生缺页, 修改pte还能修改地址的含义. 任何问题可以通过一种级别的间接访问来解决. 

#### 问题

fork会拷贝父亲的用户空间到孩子中. 如果父亲很大, 拷贝要花很长时间. 并且可能浪费-> fork后面有exec, 让孩子丢弃拷贝的内存. 只有但父亲孩子用同一页, 并且一个或者多个写的时候, 才需要复制

#### 解决方法

cow fork: 只有需要时候才给孩子拷贝物理页

cow只给孩子创建一个页表, 用户空间的pte指向父亲的物理页, 并且让孩子, 父亲的pte都不可写. 当某个进程尝试写一个cow页, cpu产生page fault. 内核来处理缺页: 为失败进程申请一个物理页, 拷贝原来页到新的页中, 修改pte指向新的页, pte可写. 

cow让释放物理页有点技巧. 一个物理页可能被多个进程页表引用, 只有在最后引用释放才释放物理页.

#### 实现

cowtest

simple测试申请大半可用的物理内存, 然后fork, 会失败, 因为没有足够空间来拷贝了

1.  修改uvmcopy: 映射父亲的物理页给孩子, 并不申请新页, pte的W位都清除
2. 修改usertrap: 处理: kalloc申请一页, 拷贝旧页给新页, 让pte_w = 1
3. 确保每个物理页只有但最后一个引用释放才释放. 为每个物理页记录ref count = 每个用户页表指向这一页. 
   1. kalloc() 时候cnt=1
   2. fork让孩子共享页表, cnt++
   3. 任何进程在页表中unmap这一页, cnt--
   4. kfree: 只有cnt==0 才释放
   5. 存放在固定大小的整数数组中, 如何index = paddr / 4096
   6. size = freelist中最高物理地址/4096
4. 修改copyout: 遇到cow页, 用page fault的方法

#### hints

1. 让你更熟悉xv6内核带吧. 不要基于lazy lab来做
2. 记录cow页. 在pte中用RSW位来记录
3. usertests也要通过
4. riscv.h 中有有用的宏和定义
5. 如果cow 缺页发生, 没物理内存了, 进程应该被杀掉



当前是mappage++, unmap--

试着只有对cow页(记录在pte的flags中!, 只有父子页共享页)的时候, mappage才++吧.

这样kvm不会全部加1, 

bug:

ls 后无法返回

cowtest: simple: usertrap(): unexpected scause 0x0000000000000002 pid=3
            sepc=0x0000000000001000 stval=0x0000000000000000

还是非法指令异常, 在pc=0x1000地方去了, 不应该去那里的

修改了进程pte在内核中的, 切换到进程时, 会刷新页表的

保护页处理错了吧, vaddr=4096以上, 不可写的保护页, 不能写的! 这个时候需要特殊处理一下!

一定要先写完所有代码再测试! 没有写copyout导致一堆bug! 一定要搞定后再去做



bug:

删除了保护页的限制, 似乎有问题? 判断条件错了?

three3 的时候有wrong content 读写不一致错误

test sbrkmuch: sbrkmuch: sbrk test failed to grow big address space; enough phys mem? 不知道过没有?
OK
test kernmem: panic: page fault walk fail



终于把cowtest跑过了! 注意要提前进行异常情况的判断! 然后再进行修改一些变量的值

服了, 有时候提前跑cowtest, 就不会出现问题了. 似乎是空间确实太少了, 没办法吧. 或许是sbrk(负数) 没有能释放空间

基本usertests都ok了, 最后有

FAILED -- lost some free pages 32336 (out of 32448)

表示有的没成功, 不知道哪里释放有问题



### lec10 多处理器和锁



### lab7: 多线程

thread结构中存储callee-saved reg

uthread.asm可以帮助debug

使用riscv64-linux-gnu-gdb调试



### lab8: 锁

现在切换到lock分支, 每次进入qemu会比较慢

使用cpus=1, kalloctest正确, 但是实验2会测试挺长时间, 10mins吧. cpus=3 kalloctest错误

会定义LAB_LOCK宏变量, 这是在哪里定义的呢?

#### memory allocator

使用kalloctest查看实现是否减少了锁争抢: 打印争抢锁次数: 由于另一个内核有锁, acquire会不断循环, 打印循环次数, 包括kmem锁和其他锁, 这粗略估计了锁竞争.

$ kalloctest
start test1
test1 results:
--- lock kmem/bcache stats
lock: kmem: #fetch-and-add 133154 #acquire() 433086 	调用13万次, 循环了43万次
lock: bcache: #fetch-and-add 0 #acquire() 2098
--- top 5 contended locks:  5个最竞争的lock: 4个proc锁, 1个kmem锁
lock: proc: #fetch-and-add 232095 #acquire() 527772
lock: proc: #fetch-and-add 150897 #acquire() 527794
lock: kmem: #fetch-and-add 133154 #acquire() 433086
lock: proc: #fetch-and-add 123069 #acquire() 527772
lock: proc: #fetch-and-add 104734 #acquire() 527772
tot= 133154 	总共13万次竞争
test1 FAIL
start test2
total free number of pages: 32499 (out of 32768)

竞争原因: 只有一个空闲内存链表--> 让每个CPU有个空闲链表. 

在本cpu空闲列表为空时, 窃取其他的cpu空闲列表.(会有竞争)

锁名字kmem开头, 每个锁要先initlock并初始化kmem开头的名字. kalloctest, usertests sbrkmuch 来测试

##### hints

1. 使用param.h NCPU
2. 让freerange把所有空闲内存都给此刻的CPU
3. cpuid函数返回当前id. 但只有关中断时候才安全, 用push_off, pop_off保护
4. snprinttf 可以参考. 所有锁名字为kmem也可以



#### bcache buffer

由于bcache->lock会保护链表, b->refcnt, b->blockno等, 但多进程访问bcache, 会有竞争.

修改bget, blease 让并发访问不同块不会导致冲突. 保持不变量: 每块只有一个备份cache.

让锁名字为bcache开头, 并初始化锁.

比kalloc更难: 是在进程间(CPU间)真正共享的. 对kalloc可以每个cpu独立锁就好, 但对bcahce不行. 用哈希表来查找缓存的块编号, 块编号作为key(可能很大), 共13个桶,  每个哈希桶有个锁.

如果有冲突, 以下情况是可行的

1. 两个进程同时使用同一个块号
2. 两个进程同时在cache中找不到, 需要找一个没用过的buf来替换
3. 两个进程同时用块并冲突(块号 hash到一个桶中, 要调整哈希桶的size)

##### hits

1. 阅读文档
2. 使用固定数量桶, 13 来减少冲突
3. 哈希表中搜索缓冲区, 找不到buf并分配, 这需要原子的, 前后加锁. 整个bread?
4. 删除buffers的链表(包括bcache.head, 不使用双向链表了?), 使用时间戳的buffers(使用的时间, trap.c 的 ticks, 加入到buf结构中). brelse不需要获得bcache 锁?, bget能基于时间戳选择最近使用的块. 每个桶存几个, 用链表
5. 可以序列化bget的驱逐顺序(会选择一个buf但lookup在cache中缺失后, 会驱逐一块)
6. 可能需要同时持有两个锁. 例如, 驱逐(替换)时: 持有bcache锁和bucket锁. 顺序来防止死锁
7. 替换一个block, 需要把struct buf从一个桶搬到另一个桶, 新的block会hash到新桶中. 需要处理: 恰好新旧block都hash到一个桶, 要防止死锁
8. debug: 实现bucket锁, 但保留全局的bcache.lock在bget的开头结尾. 最后删除全局锁. make CPUS=1 qemu来debug

用hash要13个锁, 总共也就30个buf, 直接30个锁可以吗? 感觉更简单, 也没有太大的开销吧. 失败了, 主要是处理冲突的情况还是有问题, 而且速度变得非常慢, 很奇怪.

注意先clean, 用3个cpu来测试

make clean; make qemu CPUS=3



### lab9: fs

make qemu 会先 make fs.img 执行 

mkfs/mkfs fs.img README ​\$(UPROGS) (各种程序的二进制文件)

输入一些二进制文件 + README. 输出到fs.img.  qemu这些都会用到

#### 支持大文件

当前最大268块(一块1024). inode包含12个direct直接块好 + 1个非直接块 = 12 + 256 = 268 块

测试bigfile会创建65803. 支持二重非直接 256*256 + 256+11 = 65803.

mkfs程序创建磁盘fs镜像. 

磁盘inode是dinode. 关注图8.3 

bmap() 会找到磁盘上文件数据, 理解它. 读写文件都会调用. 写文件, bmap申请一些数据块来存放文件内容. 如果需要, 会用到间接块.

bmap处理两类块号. 

1. bn是逻辑号, 文件中, 文件起始块. 
2. ip->addrs[] 存放. bread 参数. 是磁盘的块号. bmap会映射逻辑号到物理号

工作: 修改bmap. 实现双重间接块(从一个直接的改) 11个直接+1个间接+1个双重间接

hints:

1. 看明白bmap.
2. 如何用逻辑号索引双重间接
3. 如果修改NDIRECT. 修改inode中addrs[], dinode中也要改
4. 需要make fs.img!
5. bread后要brelse
6. 只有需要时候才用简介块
7. itrunc会释放文件所有块, 包括双重块

#### 符号链接

符号链接/软链接 通过路径名引用链接文件, 打开后, 内核会找到文件的链接. 硬链接会指向同一个磁盘的文件, 软链接会跨设备找(xv6其实只有单设备). 能理解路径名查找

实现 symlink(char *target, char *path) syscall. 根据路径, 创建软链接到target文件. man symlink 查看. 

hints:

1. 加syscall num. 
2. stat.h 中加新的文件类型, T_SYMLINK
3. fcntl.h 加新的O_NOFOLLOW. 会在open syscall用到, OR flag, 不要覆盖到其他flag
4. 实现symlink(target, path). 在path目录下, 创建指向target文件的符号链接. target不存在, 也可能成功. 需要在inode的data块中, 存储符号链接的目标路径. 类似link, unlink, 返回0成功, -1失败.
5. 修改open syscall 处理但路径指向符号链接. 如果文件不存在, open fail. 如果进程在open指定 O_NOFOLLOW. open只打开文件, 不追踪指向的文件
6. 如果指向的文件也是符号链接文件, 应该递归跟踪. 如果形成环路, 返回-1. 例如链接深度到10了, 报错
7. link, unlink不应该改
8. 不需要指向目录
