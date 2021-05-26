## C++ primer

### 一. 开始

#### 输入输出

1. 有4个IO: cin, cout, cerr(标准错误), clog(一般信息)
2.  while(cin >> value) 会在遇到文件结束符(EOF ctrl+z) / 输入错误时 停止.
3. 类 定义了行为, 对象可以使用的所有操作
4. 定义在函数内部的私有变量默认不初始化, 没初始化是常见bug

### 二.变量和基本类型

#### 2.1基本内置类型类型

1. 分为整型(包括字符和bool), 浮点型
2. bool, char, wchar_t, char16_t, char32_t(Unicode), short, int, long, float, double. 在前面加unsigned 变成无符号. 
3. 不可能为负数用unsigned, 整数用int, 浮点用double. 切勿混用unsigned和带符号的数!

#### 2.2 变量

1. 变量有存储空间, 数据类型(决定内存大小和分布方式, 值范围, 以及能参与的运算). 变量= 对象
2. 区别: 初始化--创建变量时候赋予一个初始值; 赋值--当前值擦掉, 用一个新值代替.
3. 建议初始化每一个变量, 定义后立刻初始化!  在第一次使用变量时候再定义它, 并给一个合适的初始值.
4. 定义: 类型说明符+变量名列表, 创建和名字相互关联的实体.
5. 声明: 让程序知道, 一个文件使用别的文件的定义函数需要包含它的声明, 或者用extern
6. 变量只能被定义一次(只在一个文件中), 但可以多次声明(需要用到的文件). extern int j; 只声明不定义

#### 2.3 复合类型

1. 引用 + 指针

2. ```c++
   int ival = 1024;
   int &refVal = ival;		// 是ival的另一个名字, 必须要初始化, 一直绑定在一起(共用一个存储空间)
   refVal = 2;		// 其实把ival赋值为2
   int &refVal3 = refVal;	// 也绑定到ival上.
   // 引用不是对象, 不能定义引用的引用
   ```

3. 指针, 引用: 实现对其他对象的间接访问. 

4. 区别: 指针本身是对象, 可以赋值和拷贝, 可以更改指的对象. 且指针不用在定义时赋初始值.

5. ```c++
   int ival = 42;
   int *p = &ival;		// 要用取地址符来指向ival
   // 指针不能指向引用(不是对象)
   *p = 43;		// 访问对象, 并修改 ival = 43
   int *p1 = nullptr;	// 空指针, 0, NULL 同理
   // 建议: 初始化所有指针. 使用未定义的指针是bug高发地! 不知道初始值也要先初始化成nullptr!
   ```

6. 注意: & 可以作为 引用/取地址符;  * 可以作为 指针/解引用符

7. 记住: 赋值永远改变等号左侧的**对象**.

8. void* 可以存放任意对象的地址, 但不能直接操作它指向的对象(没有类型, 不知道支持的操作). 作用有限: 和别的指针比较, 作为函数输入输出, 赋值给另一个void* 指针.

9. ```c++
   int *p1, p2;	// 只有p1是指针, 尽量把 *, & 和变量名连在一起!
   ```

10. ```c++
    // 指向指针的引用
    int i = 42;
    int *p;			// p 是int* 的指针
    int *&r = p;	// r 是对指针p的引用, 从右向左阅读r的定义!
    r = &i;			// r 作为引用, 被赋值成i的地址, 等价于 int *p = &i;
    *r = 0;			// 解引用r得到i, i的值改成0.
    ```

11. ```c++
    // const 来定义缓冲器大小这种变量, 防止被程序更改
    const int bufSize = 512;	// 必须立刻初始化
    
    // const 只在本文件内有效: 编译过程中把本文件内所有bufSize替换成512
    // 为了能文件间共享, 都加extern
    // file.cc 定义并初始化
    extern const int bufsize = fcn();
    // file.h 声明
    extern const int bufsize;
    // 最好用constexpr 来替代const
    constexpr int mf = 20;
    ```

12. ```c++
    // 类型别名
    typedef double wages, *p;	// wages = double, p = double*
    using SI= Sales_item;		// 别名声明, 等价
    ```

13. ```c++
    // 自定义数据类型
    struct Sales_data{
    	string bookNO;
        double rev = 0;
    };	// 定义类
    Sales_data accum, trans;	// 定义变量, 最好和类定义分开
    ```

14. ```c++
    // 编写头文件
    // 类定义在头文件中, 其他只能被定义一次的实体(const等)
    #ifndef SALES_H
    #define SALES_H
    // 定义...
    #endif
    ```

#### 2.4 指针

1. 注意指针 和指针指向的对象, 是两个独立的对象; 用顶层const 表示指针本身是常量; 底层const表示指针指向对象是常量.
2. 函数传参时候, 因为本身就不能修改实参的值, 所以有无const都是一样的, 会自动忽略const

### 三. 字符串, 向量和数组

注意头文件中不能用 using 声明

#### string

1. ```c++
   // 初始化
   string s1;		// 空串
   string s2 = "hello world";
   string s3(10, 'c');
   ```

2. ```c++
   // 操作
   cin >> s;			// 遇到空格停止, 忽略开头的空白
   getline(cin, s);	// 读取一行, 返回cin状态
   s[n];				// 返回s[n]的引用
   s += 'c';			// 后面添加字符
   s.size();			// 返回长度, size_type 无符号类型, 不要和int混用!
   ```

3. ```c++
   // 处理string中的字符c
   isalnum(c);
   ispunc(c); 		//(判读是否为标点) 
   tolower(c);		// 若c为大写字母, 变成小写; 否则原样输出
   // 建议: 使用c++版本的头文件, cname 而不是name.h
   for(auto c: str)
       cout << c << endl;		// 获取每个char
   
   // 若要修改字符, 必须用引用&, 依次绑定到每个元素中
   for(auto &c: str) 
       c = tolower(c);
   ```

4. ```c++
   // 只处理部分字符: 用下标 / 迭代器, 可以修改!
   // 下标 0, 1, ..., s.size()-1; 必须检查下标合法性, 防止越界
   for(size_t i = 0; i != s.size() && !isspace(s[i]); ++i)	
       s[i] = toupper(s[i]);		// hello world --> HELLO world
   
   // 获取子字符串
   s.substr(0, n);		// 获取[0, n) 的字符
   ```

#### Vector

1. 是可变长对象的集合, 也被称为容器.

2. c++ 有类模板, 函数模板. vector是类模板(更难). 可以把模板看做 类/函数 的生成器(创建类/函数称为实例化), 对于类模板, 需要额外信息来确定类型

3. ```c++
   vector<int> ivec;		// 空vec
   vector<int> ivec2 = {1,2,3,4};	// 列表初始化
   vector<string> articles = {"a", "an", "an apple"};
   vector<int> ivec3(10);			// 值初始化, 10个元素, 默认为0
   ```

4. ```c++
   // 最好先定义一个空vec, 再push_back 来逐渐添加!
   v.push_back(t);		// 尾部加t
   for(auto &i : v)
       i = i*i;		// 也要用引用& 才能修改元素
   // 不能用下标形式来添加元素! 只能用push_back
   // 只能对确定已经存在的元素执行下标操作, 尽可能用迭代器吧
   ```

   

#### 迭代器

1. string, vector, 所有标准库容器都能用迭代器(只有少数能用下标).

2. 类似于指针, 迭代器也提供对对象的间接访问, 有效的迭代器指向某个元素, 否则无效.

3. ```c++
   // b 表示第一个元素, e 表示最后一个元素的下一位置!(只是一个标志, 不能赋值, 解引用)
   auto b = v.begin(), e = v.end();	// 对空容器, b == e
   ```

4. ```c++
   *iter;			// 返回迭代器指向元素的引用 (解引用)
   ++iter;			// 指向容器下一个元素
   if(s.begin() != s.end()){	// 确保非空
       auto it = s.begin();
       *it = toupper(*it);		// 修改第一个字符为大写
   }
   
   // 泛型编程: 尽量用 != 而不是 < ; 因为这在任何容器都有实现!=, 不一定实现<
   for(auto it = s.begin(); it != s.end() && !isspace(*it); ++it)
       *it = toupper(*it);		// 和之前下标例子相同
   ```

5. ```c++
   vector<string> strings;
   auto it = strings.begin();
   (*it).empty();		// 判断第一个字符串是否为空
   it->empty();		// 和上一个等价
   for(auto it = strings.cbegin(); it != strings.cend() && !it->empty(); ++it)
       cout << *it << endl;	// 只是读取元素不修改, 用cbegin, cend.
   ```

6. 注意: 如果使用了迭代器的循环体, 不能向它的容器中添加元素!



#### 数组

1. 同C语言的, 大小确定, 存放对象

2. ```c++
   int *(&array)[10] = ptrs;		// 是一个引用, 绑定一个长度为10的数组, 数组存放int指针
   int scores[11] = {};			// 全部初始化成0
   ```

3. ```c++
   // 指针和数组
   string nums[] = {"one", "two", "three"};	// 使用数组时候, 编译器会转换成指针
   string *p = nums;		// 指向数组开头, 等价于 *p = &nums[0]
   // 指针也是迭代器, 用来遍历(容易出错!), 用begin, end
   int ia[] = {0,1,2,3,4};
   int *beg = begin(ia);	// 指向首元素
   int *end = end(ia);		// 指向尾元素的下一个位置
   ```

4. 尽量别用C语言风格的char[] 来当做字符串, 实在要用, 用 s.c_str (string 转换成char*)

5. ```c++
   // 可以用数组初始化vector
   int arr[] = {0,1,2,3,4}
   vector<int> ivec(begin(arr), end(arr));
   vector<int> subVec(arr+1, arr+3);		// 只拷贝1,2,3
   ```

6. ```c++
   // 数组的数组, 多维数组
   int ia[3][4] = {0};
   for(auto &row : ia)		// 外层必须是引用的
       for(auto &col : row)
           col = cnt;
   
   // 用类型别名
   using int_array = int[4];
   typedef int int_array[4];	// 等价的
   ```


### 模板

1. 泛型编程: 用一种独立于任何特定类型的方式编程; 模板是泛型编程的基础.

2. 模板是创建泛型类或函数的蓝图或公司. 库容器(迭代器/算法), 都是泛型编程的例子, 用了模板的概念.

3. 可以用模板来定义函数和类

   1. ```C++
      // 定义函数
      template <typename type> ret-type func-name(parameter list){
          // 主体
      }
      // eg:
      template <typename T> inline T const& Max(T const& a, T const& b){
          return a < b ? b:a;
      }
      ```

   2. ```c++
      // 定义类
      template <class tyep> class class-name{
          
      }
      // eg
      template <class T> class Stack{
          private:
          	vector<T> elems;
          public:
          	void push(T const&);
          	T top() const;	// 返回栈顶元素
      }
      template <class T> void Stack<T>::push(T const& elem){
      	elems.push_back(elem);
      }
      template <class T> T Stack<T>::top() const{
          if(elems.empty()){
              throw out_of_range("Stack top:empty stack");
          }
          return elems.back();
      }
      int main(){
          try{
              Stack<string> stringStack;
              stringStack.pop();
          }
          catch(exception const& ex){
          // 使用 catch(...) 能处理任何异常
              cerr << "ex:" << ex.what() << endl;
              return -1;
          }
      }
      ```


### 五. 语句

#### try 和异常处理

1. throw表达式, 引发了异常
2. try 加上多个 catch 
3. 一套异常类

``` c++
while(cin >> item1 >> item2){
    try{
    	// 正常语句, 执行语句
		if(item1 != item2)
    		throw runtime_error("data must be same");	// error 需要初始化
        cout << item1 << item2;
	} catch(runtime_error err){
        // 和用户交互的语句
    	// 提醒必须一样, 交互是否继续
        cout << err.what()	// 输出 data must be same
             << "\n try again ? enter y or n" << endl;
        char c;
        cin >> c;
        if(!cin || c == 'n') break;
	} catch(exceptino2 声明){
    	// 处理异常2
	}
}
```

通常有4个头文件

1. exception: 只有常用的类 exception
2. stdexception:
3. new 定义了 bad_alloc 
4. type_info 定义了 bad_cast



### 六. 函数

1. 局部静态变量: 能贯穿函数调用之后, 直到程序结束! 默认初始化0

2. 建议使用 引用传参 来改变函数外部的对象

3. 实参 是 形参 的初始值, 第一个实参 初始化 第一个形参 void f (int a);  int 是形参, a 是实际参数

4. 如果函数不会改变形参的值, 建议声明成常量 const, 只读不写的情况

5. 我们不能把const对象, 字面值, 需要类型转换的对象, 传递给普通的引用形参.

   ```c++
   int nums[3] = {1,2,3};
   Myprint(begin(nums), end(nums));
   
   void Myprint(const int* beg, const int* end){	// 输出[beg, end) 之间(不含end)的值
       while(beg != end)
           cout << *beg++ << endl;
   }
   ```

   ```c++
   // 数组定义成引用传参
   void print(int (&arr)[10]){ // 必须(&arr) 加上括号, 只能作用在长度为10的数组上
       for(auto elem: arr)
           cout << elem << endl;
   }
   ```

6. 函数重载: 同一作用域内部, 几个函数名字相同, 但是形参不同(类型或者数量不同)

7. 使用cassert库中的assert来调试, 如果#define NDEBUG 会忽略assert

8. ```c++
   void print(const int a[], size_t size){
       #ifndef NDEBUG
       	// _ _func_ _ 可以输出当前函数的名字
       	// _ _ FILE _ _ 表示当前文件名
       	cerr << _ _func_ _ << ":size is " << size << endl;
       #endif
   }
   ```

9. 函数指针: 使用指针替换掉函数名字就好了!

10. ```c++
    int add(int a, int b);
    int (*padd)(int a, int b);	// 指向add函数的指针
    ```

11. 











1. 理解声明要按照 由内向外, 从右向左来看!



### 七. 类







面向对象OOP:

1. 数据抽象: 把类的接口和实现分离
2. 继承: 定义相似的类型, 对其相似关系建模
3. 动态绑定: 可以忽略相似类型区别, 统一的使用他们的对象

在基类中定义虚函数, 方便继承它的派生类能够实现虚函数. 使用基类的引用(或者指针)调用一个虚函数时候, 会发生动态绑定



### 九. 顺序容器

| 名字         | 作用                                                       |
| ------------ | ---------------------------------------------------------- |
| vector       | 可变数组大小. 支持快速随机访问. 尾部之外的插入or删除可能慢 |
| deque        | 双端队列, 头尾删除/插入快                                  |
| list         | 双向链表, 只支持双向顺序访问. 任何位置插入/删除都很快      |
| forward_list | 单向链表, 只支持单向顺序访问, 任何位置插入/删除都很快      |
| array        | 固定大小数组. 不能添加/删除元素                            |
| string       | 与vector类似, 专门保存字符. 尾部添加/删除快                |



### 十一. 关联容器

1. 支持高效的关键字查找和访问, 主要的两个是

   1. map: <key, value> 关键字+值;  存储的是一个pair;  一般用于存放  名字 -> 出现次数 的映射上
   2. set: < key > 只存关键字;  一般用途: 存放不想出现的单词集合

2. 按照3种分类条件一共有8个:

   1. 是map  /  set
   2. 有无重复关键字:  有; 加上multi
   3. 是否顺序存储: 否: 加上unordered_

3. ```c++
   map<string, size_t> word_count;
   set<string> exclude = {"an", "a", "the"};
   string word;
   while(cin >> word){
       if(exclude.find(word) == exclude.end()) // find找不到, 返回末尾; 找到, 返回指向它的迭代器
           ++word_count[word];	// 对应单词计数
   }
   for(const auto &w : word_count){
       cout << w.first << "occurs" << w.second << endl;
   }
   ```

4. ```c++
   // pair使用
   pair<T1, T2> p(v1, v2);
   pair<T1, T2> p = {v1, v2};
   make_pair(v1, v2);	// 返回一个pair
   p.first; p.second;
   p1 op p2;  // > < 这些按照字典序来定义
   ```

5. 注意: 只能改变value, 不能改变关键字key, 是const的!

6. ```c++
   // 使用迭代器遍历
   auto map_it = word_count.cbegin();
   while(map_it != word_count.cend()){
       cout << map_it.first << map_it.second << endl;
       ++map_it;
   }
   // 注意: 输出是按照字典序升序排列来的!
   ```

7. ```c++
   // map 添加元素, 重复的会自动忽略(不会覆盖!)
   word_count.insert({word, 1});
   word_count.insert(make_pair(word, 1));
   // set 添加元素
   vector<int> ivec = {1,2,3,4};
   set<int> set2;
   set2.insert(ivec.cbegin, ivec.cend);
   set2.insett({1,2,3,4});
   ```

8. 尽量使用find, count操作; 如果使用下标访问, 且元素不存在, 会自动插入到map中!

9. 使用lower_bound会返回迭代器, 指向第一个有给定元素的位置; upper_bound会指向最后一个元素**之后** 的位置

10. getline(input_stream, string) 参数为输入流和一个字符串, 读取一行存到string中(不包括换行符, 但是包括前面的空格!)

11. 无序关联容器: unordered_map, unordered_set:

    1. 不是使用比较运算符来按照字节序存放元素, 而是使用一个hash函数和== 运算符
    2. 在关键字key没有序关系, 有用, 或者维护序列关系很昂贵, 也有用
    3. 使用一组桶来存储, 每个桶有0或者多个元素, 每个桶对应不同的关键字key. 如果一个桶有多个元素, 顺序访问
    4. 对于内置类型(int, point, double等), 有内置的hash模板; 但对自定义类型, 要自己写hash函数(例如pair等)