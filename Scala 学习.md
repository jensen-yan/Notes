### Scala 学习

```scala
object 	HelloWorld{
    def main(args: Array[String]): Unit= {
        println("Hello, World!")
    }
}
```

#### 简介

1. Scalable Language 是一门多范式的编程语言, 最新2.13.3
2. 特性
   1. 面向对象: 每个值都是对象, 对象的数据类型和行为 由 类和特质描述, 类的扩展: 子类继承 / 灵活的混入机制, 避免多重继承的问题
   2. 函数式编程: 函数能当成值来用, 支持高阶函数，允许嵌套多层函数，并支持柯里化。Scala的case class及其内置的模式匹配相当于函数式编程语言中常用的代数类型。
   3. 静态类型: 编译检查
   4. 扩展性: 某个特定领域需要开发语言扩展, 可以用库来无缝添加新的语言结构
   5. 并发性: Actor为并发模型, 复用线程

#### 基础语法

1. 对象, 类, 方法
2. 字段: 每个对象有他唯一的实例变量集合, 字段. 对象的属性通过字段赋值来创建
3. 类名要大写开头, 方法名要小写开头, 文件名和对象名最好匹配

#### 数据类型

1. Unit: = void, 不返回任何结果的方法的结果类型
2. Null: 空引用 / null
3. Nothing: scala类的最底端, 所有类型的子类型
4. Any: 所有类的超类
5. AnyRef: 所有引用类的基类
6. 多行字符用三个引号开头结尾
7. var 表示变量;  val 声明常量  ` val a : String = "Foo"`
8. 在没有给出数据类型下, 可以通过初始值推断 `val a = "Hello"`
9. 多变量声明 `val x, y = 100`  均为100

#### object

1. class:  无static 类型, 类的属性和方法, 必须new来调用, 有main主函数也没用
2. object: 可以用用属性和方法, 默认static, 可以直接用object名来调用属性和方法, 不用new(不支持)
3. object 中的main是函数应用程序入口
4. object, class 都可以extends父类或trait, 但是object不能作为父类, 不能extends object!

#### 循环

1. 为了支持break

2. ```scala
   // 导入以下包
   import scala.util.control._
   // 创建 Breaks 对象
   val loop = new Breaks;
   // 在 breakable 中循环
   loop.breakable{
       // 循环
       for(...){
          ....
          // 循环中断
          loop.break;
      }
   }
   ```

#### 方法和函数

1. 语义差别小, 方法是类的一部分, 函数是一个对象可以赋值给一个变量. 类中定义的函数就是方法

2. ```scala
   class Test{
       def m(x: Int) = x + 3		// 定义方法
       val f = (x: Int) => x + 3	// 定义函数
   }
   // 方法声明
   // def funcName( [参数列表] ): [返回类型]
   object add {
       def addInt(a: Int, b: Int): Int = {
           var sum: Int = 0
           sum = a + b
           return sum
       }
   }
   object Hello {
       def printMe() : Unit = {	// 无返回值, 返回类型为Unit, 可以不写
           println("Hello")
       }
   }
   ```

3. 函数可以作为一个参数传入到方法中

4. ```scala
     //定义一个方法
     //方法 m1 参数要求是一个函数，函数的参数必须是两个Int类型
     //返回值类型也是Int类型
   def m1(f:(Int,Int) => Int) : Int = {
       f(2,6)
     }
     //定义一个函数f1,参数是两个Int类型，返回值是一个Int类型
     val f1 = (x:Int,y:Int) => x + y
     //再定义一个函数f2
     val f2 = (m:Int,n:Int) => m * n
   ```

5. scala不能直接操作方法, 要操作方法, 先转换成函数

6. ```scala
   val f1 = m1 _  	// 后面加空格, 下划线
   val f1:(Int) => Int = m
   ```

7. 解析函数参数

   1. 传值调用: 先计算参数表达式的值, 再应用都函数内部
   2. 传名调用: 把未计算的表导师直接应用到函数内部(每次调用, 解释器会计算一次表达式的值)

8. 使用 def printString(args: String *)  =  {...}  星号表示可变参数

9. 匿名函数: 箭头左边是参数列表, 右边是函数体  `var inc = (x:Int) => x+1;  var x = inc(3)-1`

10. 偏应用函数: 只提供部分参数, log()有两个参数, 定义`val log2 = log(data, _:String)` 只用提供后面的参数即可

11. 闭包: 是一个函数, 

#### 数组

1. `var z = new Array[String](3)`

2. `var z = Array["he", "sd", "we"]`

3. ```scala
   val x = List(1, 2, 3, 4)	// 可重复
   val x = Set(1, 2, 3, 4)		// 不可重复
   val x = Map("one" -> 1, "two" -> 2, "three" -> 3)
   val x = (10, "runoob")		// 两个不同类型元素的元祖, 是不同类型的值的集合
   val x:Option[Int] = Some(5)	// Option[T] 表示可能包含值的容器, 也可能不包含值, 一般这样写表示可能为None
   
   val it = Iterator("Baidu", "Google", "Runoob", "Taobao")
   // 迭代器, it.next() 返回下一个元素, it.hasNext() 检测是否还有元素
   ```



#### 类和对象

1. 类是对象的抽象, 类不占据内存; 对象是具体的, 占据内存
2. 使用 new 来创建class的对象
3. 继承和Java类似
   1. 重写非抽象函数, 必须用override
   2. 只有主构造函数才可以向基类的构造函数写参数



#### scala trait(特征)

1. trait 相当于java接口, 还可以定义属性和方法的实现

2. scala的类只能继承单一父类, trait可以实现多重继承

3. ```scala
   trait Equal{
   	def isEqual(x: Any): Boolean	// 未定义方法实现
       def isNotEqual(x: Any): Boolean = !isEqual(x)	// 实现了
   }
   // 子类继承特征可以实现未被实现的方法
   class Point(xc: Int, yc: Int) extends Equal{
       val x: Int = xc
       val y: Int = yc
       def isEqual(obj: Any) = 
       	obj.isInstanceOf[Point] &&
       	obj.asInstanceOf[Point].x == x
   }
   ```

   