## chisel 学习

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

4. 一个类可以继承 多个 traits, 实现多重继承

5. 一个trait 不能有构造性的参数!

6. ```scala
   trait HasFunction{
       def myF(i: Int): Int
   }
   trait HasValue{
       val myValue: String
       val myO = 100
   }
   class Myclass extends HasFunction with HasValue{
       override def myF(i: Int): Int = i + 1
       val myValue = "Hello, World!"
   }
   
   class Myclass extends Trait1 with Trait2 with Trait3 ...
   ```

#### 抽象类

1. 类似于接口, 先声明, 之后必须要定义里面的每个变量 / 方法

2. ```scala
   abstract class MyAbstractClass {
     def myFunction(i: Int): Int
     val myValue: String
   }
   class ConcreteClass extends MyAbstractClass {
     def myFunction(i: Int): Int = i + 1
     val myValue = "Hello World!"
   }
   val concreteClass = new ConcreteClass() 
   ```

#### Objects

1. 用于单例类

2. ```scala
   object MyObject {
     def hi: String = "Hello World!"
     def apply(msg: String) = msg
   }
   println(MyObject("Hello"))
   ```

3. 伴生对象: 有同名的object, class,  用于

   1. 包含和class相关的常量
   2. 在构造器之前 / 之后 执行代码
   3. 为一个类创建多个构造器
   4. 使用new 会调用class, 不使用new会调用object

4. ```scala
   object Animal{
       val defaultNmae = "BigFoot"
       private var num = 0
       def apply(name: String):Animal = {
           num += 1
           new Animal(name, num)
       }
       def apply():Animal = apply(defaultName)
   }
   class Animal(name: String, order: Int){
       def info: String = s"Hi my name is $name, in $order"
   }
   val hopper = Animal("Hopper")
   println(hopper.info)
   ```

5. ```scala
   val myModule = Module(new MyModule) // 使用了伴生类
   ```

6. ```scala
   // case class 能访问类的参数, 也不用new
   class Nail1(length: Int)
   val nail1 = new Nail1(10)
   println(nail1.length)	// 错误, 不能访问参数
   
   class Nail2(val length: Int)	// 加了val	
   val nail2 = new Nail2(10)
   println(nail2.length)	// 可以访问参数
   
   case class Nail3(length: Int){
       require(length >= 0)	// 保证参数大于0
   }	// 不加val, 加了case	
   val nail3 = new Nail3(10)	
   println(nail3.length)	// 可以访问参数
   ```

7. chisel中模块都是继承Module, IO都是继承Bundle, 如何构建分层的硬件块, 并探索对象的重用



#### 函数式编程

```scala
// 使用def
def add1(x: Int) = x + 1
// 使用val
val add1 = (x: Int) => x + 1
val add1: Int => Int = x => x + 1
// val 可以作为参数传入函数, 
val myList = List(1, 2, 3, 4)
val myListplus1 = myList.map(add1)
```

```scala
// 匿名函数
val myList = List(5, 6, 7, 8)
myList.map(x => x+1)
myList.map(_ + 1)		// 作用同上
// 用于case来判断类型
val myAnyList = List(1, 2, "3", 4L, myList)
myAnyList.map {
  case (_:Int|_:Long) => "Number"
  case _:String => "String"
  case _ => "error"
}
// 输出: List(Number, Number, String, Number, error)

```

testOnly mini.ALUTests





