

常用命令

1. file <>  加载被调试的可执行程序文件a.out
2. b  行号/函数名/代码地址, d [编号] 删除某个断点, disable 暂停断点
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



多进程

set follow-fork-mode [parent/child]

set detach-on-fork [on/off]



layout 分割窗口

layout src 显示源代码窗口

layout asm 显示汇编窗口

layout regs 显示regs窗口

layout split 显示源代码+汇编

ctrl+L 刷新窗口

ctrl + x a 回到传统的



set confirm off  # 对每次输入yes/no 不提问, 默认是confirm on

