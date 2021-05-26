运行方法

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
```



太奇怪了吧, sh.c 在150行就能打断点, 其他地方又不行...



进程切换相关

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

