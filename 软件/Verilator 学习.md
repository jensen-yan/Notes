### 9.13



#### Verilator 学习

#### 概述

1. 把verilog设计转换成C++/ systemC模型, 编译之后就可执行. 不是传统的模拟器, 而是编译器
2. verilator类似gcc, 仿真时候, 自己写一个C++ 包装文件, main 把模型实例化为C++对象.
3. 注意当前版本: 从3.9 升级到4.1了
4. make 太慢, 改成 make -j4 (只有IO密集才能做多核优化)
5.   . ~/verilator.sh  来更换成4.1 版本    



1. 译码的src 操作数,  尝试用peekpoke进行测试, 对每个小模块
2. 对整个cpu的测试, 



​	王凯帆, 金越

1. 尽量用nemu 二进制文件, 可以直接调用具体函数. 先用来验证, 直接使用, 直接用来搭建框架. 能尽快写出rtl的代码. difftest.cpp 
2. nutshell 直接用gdb, 在代码中添加printf来看看的. 
3. 明白nutshell 的具体执行一条语句的具体. 只用明白第一条指令的如何对比, 来看具体. 实现add指令, 来具体实现来. 关注在处理器设计上, 能够明白第一条指令是如何对比的. 能尽量早的开始写rtl代码. 把verilog直接翻译成chisel代码.  
4. 写开发日志, 每天写一些.
5. 希望能尽快开始写rtl代码, 结束读代码的阶段. 创一个git的repo, 定时的commit,  让师兄来看, 来帮助我们, 把地址发给王凯帆学长或者群里.



1. 开始调试
2.  gdb /home/yanyue/nutshell_v2/NutShell/build/emu
3.  b main
4.  r -i ready-to-run/linux.bin -s 8996 -b 0 -e 0 -v ALL