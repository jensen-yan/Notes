### 手把手教你设计CPU ---- 读书笔记

#### 架构集介绍

1. 模块化指令
   1. 基本指令: RV32I / 32E / 64I 
   2. 扩展指令: M, A, F, D, C(压缩)
   3. G(通用) = IMAFD 
2. 没有硬件分支预测, 采用默认
   1. 向后跳转, sign  == 0, 默认跳
   2. 向前跳转, sign == 1, 默认不跳
3. 子程序调用
   1. 进入子函数: 用store存储当前上下文(部分通用寄存器值) 到 堆栈区, 保存现场
   2. 退出子函数: 用load  读取堆栈区的上下文,  恢复现场.  有公用的程序库, 但放弃一次读多个reg, 简化硬件设计





#### CSR 指令

1. 对csrrw, csrrwi   如果rd == zero, 不读取, 也没有读引发的副作用
2. 对csrrs, csrrc,  if(rs1 == zero), 不写
3. 对csrrsi, csrrci, if(imm == 0), 不写



1. 中断: cpu在顺序执行时候, 被别的请求打断, 去处理别的事情, 完成后, 继续执行被打断之前的.

   1. 会保存现场, 恢复现场
   2. 不支持中断嵌套 / 支持中断嵌套(按照中断优先级), 默认不支持(mstatus.MIE = 0)
   3. 外部原因导致, 大多来自外围硬件设备

2. 异常: 顺序执行时候, 出现异常时间中止当前事物

   1. 是内部原因, 例如硬件故障, 非法指令等分为sd

      

      

广义异常分为:

1. 同步异常: 异常返回后能回到之前指令, 多次异常能复现
2. 异步异常: 不能精确定位这条指令, 不能复现(外部中断)
   1. 精确异步异常: 外部中断
   2. 非精确异步异常: 读写Mem出错(时序太长)



机器模式外部中断, 只有1bit输入, 所以定义了一个PLIC平台级中断控制器, 来用于多个外部中断源的优先级仲裁和分发

也可以中mie, mip的高20位来扩展控制其他自定义的中断类型

还可以用mcause中中断>12号的值, 来自定义外部中断



1. 异常不能被屏蔽
2. 中断能被屏蔽, 由mie来做(主要的MEIE, MTIE, MSIE 分别对应外部中断, 时钟中断, 软件中断), MTIE变成0就无法响应时钟中断了
3. mip 表示中断等待, 中断来了之后某一位为1, 中断撤销后某一位=0

