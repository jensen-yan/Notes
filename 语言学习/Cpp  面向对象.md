### C++ 面向对象

1. extern int i;  可以在.h文件中声明一个全局变量, 但不是定义! 需要在某个地方定义
2. 类的成员变量是存储在对象中的
3. 对象的函数是属于类的, 只会存储一份, 不属于对象本身, 但函数是会修改对应对象的成员变量,a.f() => f(&a), 等价于把对象地址传递给函数, 对象地址指针就是this! 表示函数所属类的对象的地址指针
4. C++ 可以使用printf, 但尽量不要和cout混用, 有不同的实现方式, 可能输出会乱掉
5. g++ -m32 a.cpp  编译成32位方式, -static 变成静态编译, file a.out  可以查看a.out的格式
6. g++ a.cpp; ./a.out   使用分号来依次执行
7. 所有的成员变量都可以看成前面有默认的 this->i 



#### constructor

1. 在声明一个类并定义一个对象后, 对象的成员变量不会默认成0, 在java会这样做, C++要自己初始化(最好通过构造函数), 在visual studio中会用"烫" 来表示访问未初始化变量
2. 构造函数名和类名相同, 不会有返回值, 会自动调用
3. 可以有参数 
4. 析构函数在离开作用于时候自动调用, 销毁对象, 一般是申请过一些堆的情况, 需要销毁堆
5. 编译器会在大括号开始时候, 分配括号中所有对象的空间; 但是构造函数会在被定义对象时候才运行

```c++
int c[] = {1,2,3,4};
int sz = sizeof(c) / sizeof(*c);	//能算出大小
struct X = {int i; float f;};
X x1 = {1, 2.2};	// 自动对应
X x2[2] = { {1, 1.1}, {2, 2.2} };
// C++ 中 struct 和class很像
struct Y {int i; float f; Y(int a)};	// 后面有构造函数
Y y1[] = {Y(1), Y(2), Y(3)};	// 定义3个对象
 
// default constructor 默认构造函数, 没有参数, 可以是自己写的
// 编译器也会给一个默认的, 叫自动默认构造, 也没有参数
Y y2[2] = {Y(1)};	// 错误的!, 只有第一个有参数, 第二个没有, 会找不到对应的构造函数

// 初始化列表
class A{
    int *p;
    int i;
    A():p(0), i(10) { ...}	// 使用冒号, 括号列表来初始化
    A(int ii, int *pp): i(ii), p(pp){}
}
// 和构造函数内部区别: 初始化列表会早于构造函数, 声明时候就做了, 能初始化任意类型数据
// 而构造函数: 先调用默认初始化, 然后赋值

// 强烈建议: 对所有成员变量, 都使用initial list 列表来初始化!
```



####  new delete

1. 之前都是用的静态变量定义, 直接Book  book;
2. 而像C想要动态分配空间, 在堆中, 像 malloc这种, 需要使用new Stash, new int[10]
   1. 从堆中分配空间
   2. 调用构造函数
   3. 返回分配对象地址
3. delete 释放空间: delete p; delete [] p (new时候有方括号, delte也有, 表示释放一个数组的空间, 并对每个元素调用析构函数)
   1. 先调用析构函数
   2. 释放空间
4. 对简单new, 只用c++库来完成, 不需要os参与分配内存

```c++
int *p = new int[10];
p++;	// ++之后, delete 找不到, 失败!
delete [] p;
Student *r = new Student[10];
delete r;	// 只会调用第一个对象的析构, 收回所有空间
delete [] r;	// 调用每个对象的析构
```

注意: delete一个空指针是可行的, 但尽量用 if(p) delete p;

如果new之后没有delete, 对小程序是正确的, 但如果程序要运行很长时间, 会导致内存泄露.



#### 访问限制

1. public
2. private: 只有这个类的成员函数才能访问, 可以是变量or函数
3. protected: 只有这个类和子类才能访问

注意: 对一个类的两个对象, 某个对象可以访问另一个对象的private变量. private只对类是有效的. 只在编译时候有作用!

c++ 的oop特性只在编译时候有作用, 运行时就没用了

4. friend: 声明别人是自己的friend后, 别人可以访问我的private东西, 别人可以是函数or类, 会破坏oop, 在运算符重载使用

```C++
struct X;	// 只是前向声明
struct Y{
    void f(X*);
}

struct X{
private:
	int i;
public:
    // 4个朋友, 都可以访问private i
    friend void g(X*, int);
    friend void Y::f(X*);		// 必须把Y的声明先写了
    friend struct Z;
    friend void h();
}
```

1. 注意朋友只是单向的, X声明Y是朋友, Y能用X的, X不能用Y的
2. 授权是在编译时候检查
3. class struct区别, 在没有写访问属性时候时
   1. class   默认private
   2. struct 默认 public

#### 对象组合

1. oop三大特性: 封装, 继承, 多态性 
2. 继承: 软件重用(一种梦想)
3. 组合composition : 用已有对象组合出新的对象, 两种方式
   1. fully: 那个别人对象是我的一部分(生娃之前), 对象是一部分
   2. by reference: 那个别人对象我知道在哪里, 可以访问调用, 但不输入我.(生出的小孩), 使用子对象的指针
4. 这是内存模型: 如何访问到对象. java没有fully, 只能指向它访问, 简单
5. 对组合成的类, 在构造函数中用初始化列表调用成员类的构造函数



#### 继承

1. 继承(inheritance)用已有的类, 进行改动, 得到新的类. (虚的类来定义)
2. 能够复用
   1. 成员变量
   2. 成员函数
   3. interface接口(public的部分)
3. student 继承 person
4. <img src="/home/yanyue/.config/Typora/typora-user-images/image-20210512092129994.png" alt="image-20210512092129994" style="zoom:50%;" />
   1. student是子类, Person是父类, 超类
   2. student比person扩充了更多数据
   3. <img src="/home/yanyue/.config/Typora/typora-user-images/image-20210512092421812.png" alt="image-20210512092421812" style="zoom: 33%;" />
   4. 父类在上, 子类在下
5. 父类的私有变量, 子类不能直接修改, 只能通过父类的public函数来改
6. 使用惯例
   1. 所有数据用private
   2. 让main使用的东西用public
   3. 只给子类使用的, 用protected, 用来让子类修改父类的private data



#### 父类子类关系

1. 函数重载, 名字相同, 参数不同, 且能相互调用

2. ```c++
   inline void Employee::print(ostream& out) const{
       out << m_name << endl;
       out << m_ssn << endl;
   }
   inline void Employee::print(ostream& out, string &msg) const{
   	out << msg;
       print(out); 	// 调用上一个同名函数, 尽量复用代码!
   }
   // 如果子类也有同名的print, 调用父类print要加上父类的前缀Employee::print(out)
   ```

   ```c++
   class A{
   public:
       int i;
       A(int ii):i(ii);	// 没有默认构造函数
   }
   class B : public A{
   public:
   	B():A(10){}    // 必须先初始化父类的构造
   }
   ```

3. 顺序: B继承A, 先初始化A, 然后B; 先析构B, 在A

4. 同名函数隐参: 如果父类, 子类有同名函数, 那么在子类中, 父类的函数被隐藏了, 需要加上父类::print, 才能调用. (只有C++ 这样做, 其他语言都不是这样的. 对于java, 会override父类的函数, C++中, 这个是无关的)



### chap 7

#### 函数重载和默认参数

1. function overload: (不是override!) 函数有相同名字, 不同参数列表

2. 对于名称相同, 参数相同, 返回类型不同, 编译器无法分辨! 返回类型不能构成重载

3. defualt argument, 且必须在最右边, 这个值写在.h

4. ```c++
   int harpo(int size, int initQ = 0);
   // 只能在声明中, 一般写在.h中, 不能在定义这里.cpp 不能重复
   int harpo(int size, int initQ){	// 定义
       cout << size << initQ << endl;	
   }
   // 现在版本好像可以了
   ```

5. 是编译器决定的, 不是运行时的.

6. 尽量不要用default value, 很不安全, 代码不容易阅读, 坚决不用!

7. 软件工程中, 凡是少写代码的, 一般不安全, 不要用



#### 内联函数

1. 调用函数有额外开销 overhead: 进出堆栈
   1. 把函数参数入栈
   2. 把返回地址入栈
   3. 运算得到返回值
   4. 把所有本地变量清空, pop, 跳转到返回地址
2. inline 函数, 不会调用, 直接把代码嵌入对应位置, 还是有独立空间的, 类型检查这些是编译器做的, 运行时不会有额外开销
3. 注意在声明和定义都要重复写inline!
   1. .cpp 是用来产生函数的, 编译器会让函数变成inline
   2.  .h是用来告诉调用者, 这个函数类型, 告诉调用者这是inline的
4. 注意
   1. 对一般类/ 函数, .h 放声明, .cpp放定义
   2. 对inline 函数, 函数体定义也是声明, 直接把函数体都放在.h 中!
5. 权衡
   1. 使用inline减少函数调用开销
   2. 但是如果函数被多次调用, 会导致被插入到很多地方, 程序会变大, 牺牲代码空间, 以空间换时间, 大多情况下, 是值得的
   3. C的宏定义不能做类型检查, inline更安全
6. 对于一些较大的函数，递归的函数, 编译器可能拒绝
7. 对于类的成员函数, 如果写在声明中.h中, 默认是inline的
8. 对于比较小的函数, 或者有循环的函数, 可以用inline



#### const

1. 定义后不能修改了

2. const仍然是一个变量, 在内存中有空间的; 而常量只是编译过程中有, 记在编译表中的一个int, 运行中没有

3. ```c++
   const int bufsize = 1024;	// 定义确定
   extern const int bufsize; 	//
   
   cin >> x;
   const int size = x;		
   double classAverage[size];	// error, 编译时候还不知道, 不知道要分配多大的空间给它
   ```

4. 指针和const

5. ```c++
   char * const q = "abc";	// 指针是const, 对象不是, const和指针挨的很近!
   *q = "a";
   const char * q = "ABC";	// 指针不是, 对象是const
   q++;
   *q = 'b';	// error, 不能通过指针修改const, 并不意味对象变成const了, const判断是在编译时候判断的
   // const 在 * 前面: 对象是const; * 后面: 指针是const
   // 从右向左读: char* const 说明const修饰指针*, const char* 说明char 对象是const的
   ```

6. <img src="/home/yanyue/.config/Typora/typora-user-images/image-20210513155908383.png" alt="image-20210513155908383" style="zoom:50%;" />

7. ```c++
   char *s1 = "hello world";	
   // warning: 把常量: 代码区的 "hello world" 一个const char* 赋值给 char* 了, char * s 直接指向代码段了
   s1[0] = 'B'; 	// segment fault 不能修改代码区
   char s2[] = "hello world"; 	// 正确!, 且可以修改s[0]
   // 先在堆栈中分配一个数组存放s[], 然后 = 其实是拷贝了
   ```

8. ```c++
   void f(const int *x);	// 函数保证不会对*x内容修改
   ```

9. 函数传参传入整个对象很大开销, 传指针好, 但为了防止函数对对象进行修改, 可以加上const!

10. ```c++
    const Person person;	// 整个对象为const, 成员变量不能修改
    // 为了保证成员函数不对成员变量修改
    int Person::get_name const{	// 在函数后面加上const, 声明定义都要有, 意味this是const
        return this->name;
    }
    ```

11. ```c++
    class A
    {
        void f() { cout << "f()" << endl;}
        void f() const { cout << "const f()" << endl;}
        // 其实是函数重载, 第一个参数为A* this; 第二个函数参数为const A* this. 由于a是const, 选择第二个函数
    };
    
    int main(){
        const A a;
        a.f();	// 输出 const f();
    }
    ```

12. 如果成员变量是const, 必须用初始化列表先初始化

13. 对于const变量, 不能成为数组大小



#### 引用

1. 引用  reference

2. c++ 复杂在于: 提供太多内存模型

   1. 太多可以放对象的地方: 堆, 栈, 全局数据区
   2. 太多可以访问对象的方式: 用变量直接存对象, 用指针访问, 用引用访问
   3.  3*3 = 9种组合方式

3. ```c++
   char c;
   char* p = &c;
   char& r = c;	// 定义就要初始化, r是c的别名, 右边必须是实际变量名
   // 如果在参数表or成员变量中的引用, 可以没有初始化, 由调用函数or构造函数初始化
   // 对本地变量和全局变量, 必须要初始化
   
   int x = 47;
   int &y =x;	
   y = 18;	// 把x也修改成18了
   // 字面理解: y就是x的别名, 和x绑定了, 对任何y, 可以替换成x
   const int&z = x;	// z是x的别名, 本身不可改变, const说通过z不能修改x
   
   void f(int &x);	// x就是y, 可以修改外面的y, 很邪恶
   f(y);	
   f(i * 3);	// error, 绑定对象必须有位置! i*3是中间变量, 还没名字没位置
   
   int g;	// 全局变量
   int& h(){
       int q;
       // return q;	// error, 返回后q丢失, 不能作为绑定对象传出去
       return g;	// 返回全局变量q的引用
   }
   int main(){
       h() = 16;	// 函数作为左值! 把16赋值给h返回对象g, 让g = 16
   }
   ```

4. 绑定是运行时不能改变的, 指针在运行时可以改变

5. 其实引用是依赖于const指针实现的, 目的在于: 让函数调用少一点星号

   1. 对于java: 内存模型简单: 对象只能放在堆中, 只能通过指针访问对象, 告诉别人这是引用, 其实是指针, 但是没有星号
   2. 和c++指针区别: 没有星号, 且不能运算
   3. 其实没必要有那么复杂的内存模型, c++能做的java都能做

6. ```c++
   int x, y;
   int &a = x;
   int &b = y;
   b = a;	// 其实让y = x
   // 没有引用的引用
   ```

7. 限制:

   1. 不能有引用的引用
   2. 引用能绑定指针, 但是指针不能指向引用 int& * p 是错的, int *&p 正确
   3. 没有引用的数组

8. 