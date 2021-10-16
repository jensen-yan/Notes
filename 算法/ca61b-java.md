### ca61b-java

#### java基础

1. javac HelloWorld.java 编译产生 HelloWorld.class文件

2. java HelloWorld  运行（必须有class文件）

3. ```java
   public class HelloWorld {
       public static void main(String[] args){
           System.out.println("Hello World");
       }
   }
   ```

4. class 能经过静态类型检查，对分布式系统好用。 对机器也更容易执行

5. java有静态类型，使用前需要先声明类型。每个变量的类型一旦定义后不能改变！和Python有很大不同

6. 所有代码必须在class类中！文件名和类名相同

7. 所有函数在类中，method， 用public static int larger(int x, int y){ } 这样定义。 返回值只能有一个。

8. ```java
   int[] numbers = new int[3];
   int[] num = new int[]{4, 7, 8};
   int len = num.length;
   ```

9. ```java
   for(int i = 0; i < a.length; i = i + 1){
   }
   ```

10. 



#### idea

1. alt+enter 快速修复错误
2. ctrl+shift+A or 两次shift 打开搜索。 ctrl+N 可以缩小范围
3. 如果想看某个类在库中具体实现，把右上角项目文件改成所有位置， ctrl+Q 可以预览文档
4. ctrl + 空格  基本补全。 ctrl+shift+enter 补全语句
5. ctrl+W 可以扩大代码选区，ctrl+shift+W可以缩小
6. shift + 上箭头 可以选中多行
7. ctrl+ D 可以复制粘贴选中行
8. alt+shift + 上 上移整行。 ctrl+shift+上  （光标在方法上） 上移整个方法
9. ctrl -  折叠代码， ctrl+shift+ -   折叠全部代码 ctrl = 展开
10. ctrl+ alt + t  环绕代码添加模块



#### 类

1. 类中不一定要有main，但想运行某个类，需要定义main函数
2. 类中分为static / non-static(instance实例) 对象or函数。 后者表示每个类不同，根据初始化方法不同，有自己的方法，不是类通用的。
   1. static方法不能访问instance数据
   2. x = Math.round(5.6) 对static方法，可以通过类名调用，不用先定义Math m = new Math(); x = m.round(5.6)。 更加方便调用。
   3. 对static函数和数据，尽量使用类名直接访问



#### 排序

1. 对于字符串数组比较用==， 是错误的，java只会判断是否是相同的地址！

2. 对单个字符串比较可以用==吧

3. 使用jUnit 可以节省编写测试时间

4. ```java
   org.junit.Assert.assertArrayEquals(expected, input);
   // 需要下载junit包到外部库中！
   ```

5. 插入排序，找到最小元素，插入最前面，再从后面的找最小元素

6. 尽量每次写一个函数就测试一次，让每次关注点都聚焦在一个小函数中！并且在以后重构代码时候更有信心！

7. 在每个测试函数前加上 @org.junit.Test  （注意库函数要包含hamcrest, junit, 每个测试函数不能是static的）， 就可以直接执行所有测试函数

8. 注意类名尽量用大写字母开头

9. import static org.junit.Assert.* 后， 之后调用就可以省去前缀了(有static！)

10. 面向打分系统去写代码是非常低效的，浪费时间！去使用单元测试，每写一个小函数就写一个测试程序，这样能增强自信心，也能关注点分离，同时能理解清楚你要写的程序功能是什么样的



#### List

1. java的8种基本数据类型： byte, short, int, long, float, double, boolean, char

2. 其他所有类型（例如数组）， 都是reference type.  初始化为null 或者用new 来初始化，存放的是引用对象的地址！

3. 基本类型 x = y 会拷贝值。 引用类型拷贝 x = y 只会拷贝引用（地址）， 指向同一个对象！

4. 参数传递：对基本类型，传递值；对引用类型，传递地址。easy！

5. ```java
   int[] a;	// 声明有这个引用对象
   a = new int[]{1,2,3,4};		// 创建数组对象，把地址传递给a
   ```

6. 开始构建list：能无限增大；而java的array总是固定大小的



#### Array

1. 初始化

2. ```java
   int x[], y[];
   x = new int[3];		// 默认初始化为0
   y = new int[]{1,2,3,4};
   int z[] = {5,6,7,8};
   
   // 复制array 值
   System.arraycopy(b, 0, x, 3, 2);
   // python x[3:5] = b[0,2], 从b的0号复制两个值到x的3号
   
   // 二维数组
   int[][] matrix = new int[][]{
       {1,2}, {3,4}
   }
   ```

3. array 和 class区别： 都能用固定大小的空间才存储数据

   1. array的数据都是同一类型，class可以不同
   2. array能在运行时索引下标对应的值，class不能

4. java 不允许创建泛型类用` items = (Item[]) new Object[100]; `  来解决



#### 测试

1. 使用StdRandom.uniform(0, 4) 来生成0，1，2，3 的随机值

2. 使用assertEquals(a, b) 来比较

3. ```java
   import org.junit.Test;
   import org.junit.Assert.*;
   
   public class Test1{
       @Test
       public void randomTest(){
       	int opNum =  StdRandom.uniform(0, 4);
           if(opNum == 0){
              // addLast
              int randVal = StdRandom.uniform(0, 100);
              AL.addLast(randVal);
              BL.addLast(randVal);
              assertEquals(a, b);
           }
       }
   }
   ```

4. 添加条件断点 a == 12

5. 随机测试可能有用，还是试试，500， 5000次

6. 对随机测试出错，可以在断点，任何异常，条件，`this instance of java.lang.ArrayIndexOutOfBoundsException` 来定位到出错时候的栈帧。
