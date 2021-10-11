

#### 常用命令

1. file <>  加载被调试的可执行程序文件a.out
2. b  行号/函数名/代码地址, d [编号] 删除某个断点, disable 暂停断点. tbreak暂时断点
3. s 执行并进入函数调用, n 不进入函数;  si, ni 针对汇编程序
4. p 打印变量
5. watch 变量 监视, 一旦变化就停止
6. display /i \$pc 每次查看当前指令汇编代码, /i为16进制, $pc表示当前汇编指令
7. i info 显示信息  i r, info all-registers, p /x $mepc
8. help 查看各类
9. u until 跳出当前循环
10. call 函数(参数) 调用
11. where/bt(backtrace) 显示堆栈
12. show args / set args 修改参数
13. bt 查看堆栈信息
14. info frame  查看堆栈更详细信息
15. frame 0/1/2 : 切换到不同层级的堆栈, 再使用info frame 查看信息
16. x/nfu addr : 查看内存地址的值
    1. n: 个数
    2. f: format, 包括x(hex), d(decimal), f(float), a(addr), i(instruction),c(char)
    3. u: 按照多少字节作为一个内存单元, 默认是4, g=8, b = 1
17. advance 18: 运行直到18行



#### 多进程

set follow-fork-mode [parent/child]

set detach-on-fork [on/off]



#### layout

layout 分割窗口

layout src 显示源代码窗口

layout asm 显示汇编窗口

layout regs 显示regs窗口

layout split 显示源代码+汇编

ctrl+L 刷新窗口

ctrl + x a 回到传统的



set confirm off  # 对每次输入yes/no 不提问, 默认是confirm on



#### core dump

参考https://blog.csdn.net/sunxiaopengsun/article/details/72974548

对出现segment fault的程序, 会生成core文件, 是运行崩溃时刻的内存快照, 存储内存, 寄存器状态, 运行堆栈等信息, 本质还是二进制文件, 可以用gdb, elfdump, objdump看

如果找不到core: 用ulimit -c unlimited 设置无限大, 一定产生

gdb program core	打开程序的core文件

bt	查看堆栈信息



#### vscode

使用-exec 执行gdb命令

watchpoint用: fp,x 可以显示fp变量的16进制内容, fp,o为八进制
