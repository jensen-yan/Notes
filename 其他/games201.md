### 引言

物理引擎: 对物理系统近视模拟(刚体, 软体, 流体 ), 模拟真实世界

分析应力分析, 电影特效, VR/AR, 训练机器人(无人驾驶), 

游戏(angry birds, besieged, 塞尔达传说, ori and the will ) 

yuanming-hu.githubg.io/FLAT   gear   fluid  smoke 

MLS-MPM, CPIC siggraph 2018

正常模拟: 给定初始, 模拟10s后的结果

可微模拟: 初始状况的扰动, 对应结果是怎样的

和大家一起解决issue, 是非常快乐的事情

目录:

离散化, 写求解器, 提高生产力, 提升性能, 硬件体系结构, 数据结构, 并行, 求导数



### taichi语言

高性能, 领域特定, 基于python的计算机图形学的语言

用于写图形算法的, 可用于多个机器(只要装有)

面向对象的, 自动并行的, megakernel

稀疏数据结构, 可微编程

#### 初始化

ti.init(arch=ti.gpu) try cuda/metal/opengl



#### data 

支持signed ti.i8/i16/i32/i64	常用i32, f32

unsigned ti.u8/u16/u32/u64

float			ti.f32/f64

还不支持bool, 用i32来存比较值

张量tensors: 高维度数组, 0, 1, 2维

而张量内部元素也可能是张量, 严格区分张量和矩阵!t

tensor内部元素, 分为 var, ti.Vector, ti.Matrix(), 使用a[i,j,k]来访问, 不支持指针, 返回对应的值



#### kernel

用来计算的函数

会及时编译, 变成高性能的运行

静态类型的, socpe是block的

前面加@ti.kernel, 会把下面的函数变成高性能的函数

```python
@ti.kernel
def calc() -> ti.i32:	# 有返回类型
    s = 0
    for i in range(10):
        s += i
    return s
print(calc())
```

函数@ti.func

func可以被ti.kernel 调用, 不能被python调用

func不能直接被python调用, kernel可以

```python
@ti.func
def mul3(i):
    a  = i * 3
    return a
# 强行inline, 不能递归, 只能一个return
```

函数 ti.sin, cos, atan2, cast, sqrt, floor, ceil, inv, tan, tanh, exp, random, 

abs, int, float, x ** y











