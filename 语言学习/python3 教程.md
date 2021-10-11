## python3 教程

多使用help来查找资料!

使用dir(\__builtins__) 查看所有内置变量 

python最重要的是缩进! 让代码整洁漂亮. 正确地方用冒号, 换行自动缩进

#### 输入输出

1. 使用input(), 默认返回string

2. ```python
   #!/usr/bin/env python3
   number = int(input("Enter an integer: "))
   
   chmod +x testhundred.py 
   #输出, 用{}  + format
   print("Year {} Rs. {:.2f}".format(year, value))
   ```

3. `print()` 除了打印你提供的字符串之外，还会打印一个换行符，所以每调用一次 `print()` 就会换一次行，也可以通过 `print()` 的另一个参数 `end` 来替换这个换行符, 不换行只用空格替代换行符

4. Python 中有关下标的集合都满足左闭右开原则，切片中也是如此，也就是说集合左边界值能取到，右边界值不能取到。a[0,5] 表示【0, 5)

5. for i in range(5):  print(i) 可以输出数值序列;        for i in arr:  直接遍历每个元素

6. 注意要用 True ! 不是true, 首字母大写



#### 变量和字符串

1. 一般来说变量存在内存中, 给他赋值
2. 但是python不是把值存在变量中? 变量没有类型, 只是相当于一个名字用于标志赋的值在哪
3. 注意变量必须要赋值, 然后才能使用
4. python中单引号, 双引号几乎没区别.
5. 字符串中有引号, 用转义\ , 或者使用双引号
6. 可以使用str = r'C:/now' r加上引号表示原始字符串,  末尾不能是反斜杠
7. 多行字符, 用三重引号



python的 and 和C的&& 不同

1&&3 = 1, 1 and 3 = 3



#### 函数

1. 纯函数: 有一些输入及输出的函数. 调用后返回值, 没有其他效果. abs(-2) 就是纯函数

2. 非纯函数: 调用还有副作用: 例如print会返回None, 同时屏幕输出字符

3. from operator import add, mul, sub  # 引入中缀表达式

4. 如果变量定义在函数体外面(不在当前函数栈帧中), 成为全局变量. 想要在函数体内部修改全局变量, 加上 nonlocal 变量 声明一下, 这个变量是全局的, 然后再修改

5. mutability 易变的: 包括列表, 字典. 状态可以变化.

6. not mutable 不可变的: 包括元祖, 函数

7. ```python
   is 类似 ==: 判断是否是同一个对象. == 是同样的值就可以
   2+2 is 3+1  -> True
   lst1 = [1,2,3,4]
   lst2 = [1,2,3,4]
   lst1 is lst2  -> False
   lst1 == lst2  -> True
   
   >>> lst1 = [1, 2, 3, 4]
   >>> lst2 = lst1
   >>> lst1.append(5)
   >>> lst2
   [1, 2, 3, 4, 5]
   >>> lst1 is lst2
   True	# 仍然是同一个对象指针
   ```

8. 



#### 数据类型

1. 分成整型, 包括很长的整数
2. 布尔: True = 1, False = 0
3. 浮点
4. e: 2.5e-7, 1.5e4= 1500.0
5. 字符串, str() 转换成string, 注意不要用变量名用str!

可以用isinstance(3, int) 来判断3的类型是否是int



#### 分支循环

1. 是所有程序的核心内容!

2. python没有switch, 只有if...  elif ...  else

3. 可以很好防止悬挂else! 必须缩进强制对齐

4.  small = x if x < y else y   三元操作符

   1. small = x if (x < y and x < z) else (y if y<z else z)

5. assert 3 > 4, 如果后面假, 崩溃. 能尽早发现bug

6. x, y, z = y, z, x   可以直接交互3个变量的值!

7. 资格运算in.    (name = "晏悦")  '晏' in name => True 

8. for x in 列表; 

9. for index, x in enumerate(list)  可以获得下标index

10. print

11. range([start], stop [, step = 1]), 不包括stop

12. ```python
    def print(self, *args, sep=' ', end='\n', file=None): 
    # 默认end为'\n' 换行
    ```

13. break是跳出当前循环, continue是进入下一次循环



#### 列表

1. 可以使用append, insert 来操作【1,2,3】

2. 使用pop, append(没有push) 可以当做栈来用, pop(0) 可以弹出栈底元素

3. ```python
   squares = [x**2 for x in range(10)]
   squares = list(map(lambda x: x**2, range(10)))
   ```
   
4. a = list() 似乎默认就是全局变量, 可以直接在函数中对其修改. 对于普通变量, 需要在函数中添加global 标志, 才表明这个变量是全局的!

5. 栈 先进后出(push = append, pop = pop)

6. 队列 先进先出: import queue; q = queue.Queue();  (进: q.put; 出: q.get)

7. 列表解析式(list comprehensions)

8. ```python
   [<expresion> for <element> in <sequence> if <conditional>]
   [i**2 for i in [1,2,3,4] if i % 2 == 0] >>> [4, 16]
   ```



#### 迭代器

1.  iter(iterable) : 输入一个可以迭代的对象(list等), 返回一个迭代器
2. next(iter) : 返回当前迭代器指向的值, 迭代器后移

```python

   lst = [2,3,4]
   a = iter(lst)
   next(a)		# 2
   
   r = range(3,6)
   ri = iter(r)
   for i in ri:
       print(i)	# 3,4,5
   for i in ri:
       print(i)	# None 因为上一个循环已经让ri到末尾了
```

3. 内置的函数: 大多只有需要用才算, lazy的
   1. map(func, iterable):  让列表中每个元素通过func映射一次
   2. filter(func, iterable): 过滤出func(x) 为真的元素 x
   3. zip(first_iter, second_iter): co-indexed(x, y) pairs
   4. reverse(sequence): 反序
   5. list(iterable)
   6. tuple(iterable)
   7. sorted(iterable)
   
4. ```python
   # 通用处理方法
   iterator = iter(iterable)
   try:
       while True:
           elem = next(iterator)
           ...
   except StopIteration:
       pass
   
   it = iter([3,4,5,6])	# iter把列表转换成迭代器
   ```
   
5. iterable 像一本书(可以自由翻页), iterator 像书中的书签(或者一个传送带不断扔东西下去). 一本书可能有多个书签, 彼此独立. next让书签后移一页

6. iterator 是 iterable, 可以不断遍历. 反之不成立


#### 生成器

generator 是一个特殊的迭代器, 由generator function(生成器函数) 返回. 类似function, 但是使用yield

1. ```python
   # 使用yield 返回, 而不是return. 
   # 可以yield 多次, 通过next来依次执行
   def plus_minus(x):
       yield x
       yield -x
       
   next(t) # 3
   next(t) # -3
   # 只有next执行时, 才正式开始执行函数体内容, 直到yield语句, 又停止执行, 等待下一次调用next, 能记住函数状态!
   
   ```

2. generator 也可以从迭代器生成

3. ```python
   list(a_then_b([3,4],[5,6]))
   def a_then_b(a, b):
       yield from a	# 从iterable/iterator 生成所有值
       yield from b
       
   def countdown(k):
       if k > 0:
           yield k		# 递归yeild
           yield from countdown(k-1)
   ```





#### 元组

1. tuple由逗号分隔   `a=12, 2, 1`  , 是不可变的类型, 不能添加删除  `a=12,` 表示只有一个值
2. 使用`type(a), len(a)` 



#### 集合

1. set是无序不重复元素的集合, 可以关系测试, 消除重复元素 `a={23,"se", 2}  a=set()`, 注意`a={}` 表示创建空字典!
2. `a = set('abracadabra')` 可以自动去重复 ` a ={'a', 'r', 'b', 'c', 'd'} `
3. 操作: ` a-b  a| b  a &b  a^b`
4. 'a' in set('acasdsfa') 查找元素是常数时间的, 不像list中查找需要线性时间! 
5. a.union({'one', 'two'})
6. a.intersection({'one', 'two'})   求交集. 不会改变原来的集合



#### 字典

1. 是无序的键值对 `da = {12:3, 23:"a"}`

2. 在python 3.6 之后,  存储顺序是按照添加顺序来存储的, 迭代器也是如此顺序

3. dict(ar) 把元组转换为字典

4. `for x, y in data.items():` 来遍历字典

5. 往字典中的元素添加数据，我们首先要判断这个元素是否存在，不存在则创建一个默认值。如果在循环里执行这个操作，每次迭代都需要判断一次，降低程序性能。我们可以使用 dict.setdefault(key, default) 更有效率的完成这个事情

6. 试图索引一个不存在的键将会抛出一个 keyError 错误。我们可以使用 dict.get(key, default) 来索引键，如果键不存在，那么返回指定的 default 值。

7. 在遍历列表（或任何序列类型）的同时获得元素索引值，你可以使用 enumerate()。`for i, j in enumerate(['a', 'b', 'c']):`    需要同时遍历两个序列类型，你可以使用 `zip()` 函数。`for x, y in zip(arr1, arr2)`

8. ```python
   # 对每个字符计数
           counts = {}
           for c in s:
               # 如果不存在, 申请一个
               if counts.get(c, 0) == 0:
                   counts.setdefault(c, 1)
               else:
                   counts[c] += 1
               # 存在, 加1
   # 简单方法2
   count = counts.get(c, 0)
   counts[c] = count + 1
   ```

9. dict[key]  等价 dict.get(key)

10. ```python
    k = iter(d.keys()) # or iter(d)  按照keys来作为迭代器
    v = iter(d.values())	# 按照value
    i = iter(d.items())		# (key, value) 一对
    ```




#### 面向对象

1. 每个对象通过收发消息来交流. 每个对象都有自己的状态, 成员, 和函数.

2. python每个元素都是一个对象

3. ```python
   class Account:
       interest = 0.02	# 类的成员(attribute), 所有类的实例所共享的
       def __init__(self, account_holder):
           self.balance = 0
           self.holder  = account_holder
       def deposit(self, amount):
           self.balance += amount
           return self.balance
       # 几乎每一个method 的第一个参数都是self
   
   a = Account("Jim")
   a.deposit(100)
   getattr(a, 'balance')	# a.balance 等价
   
   ```
   
4. 当class被调用时:

   1. 创建一个新的实例
   2. \__init__(self, args...) 会自动被调用, 构造函数

5. 每个object有自己的identity, 用is可以判断. 区别 c = a, 只是把一个object绑定到另一个变量c上了, 没有创建新的object, c is a --> True

6. object + function  = bound method.  区别: 类里面的函数叫做method 方法

7. 区别对象的成员(self.balance ), 类的成员(interest) . 会首先找对象成员, 找不到再找类成员, 再找其基类的类成员(类成员会不断继承)

8. 如果给对象成员赋值, 找不到, 会新增一个成员并赋值

9. 继承(inheritance): class \<name> (\<base class>): 

10. ```python
    class CheckingAccount(Account):
        withdraw_fee = 1
        interest = 0.01
        def withdraw(self, amount):
            # 调用父类的方法并在子类中覆盖它
            # 防止重新实现一遍难以修改
            return Account.withdraw(self, amount + self.withdraw_fee)
    ```

11. 查找类中名字:

    1. 如果是类的成员, 返回
    2. 否则, 在基类中找, 递归找

12. 基类的成员没有全部拷贝进入子类中! 只有子类用到了再去基类中找

13. 区别继承, 组合 的使用场景

    1. 继承: A is a B. 特殊卡是一种卡
    2. 组合: A has a B. 汽车含有轮胎

14. ```python
    class Bank():
        def __init__(self):
            self.accounts = []
            
        def open_account(self, holder, amount, kind=Account):
            account = kind(holder)	# 组合
            account.deposit(amount)
            self.accounts.append(account)
            return account
        
        def pay_interest(self):
            for a in self.accounts:
                a.deposit(a.balance * a.interest)
    ```

15. 多重继承class \<name> (\<base class1>, \<base class2>): 



#### 接口

1. python中, 所有object产生两种string表达

   1. str  类似人类表达, 更加直观,print(...) 输出
   2. repr  用于python解释器, 交互器中输出

2. 多数时候str , repr 相同 

3. ```python
   # 实际定义
   def repr(x):
       return type(x).__repr__(x)	# 找到x的类, 调用类方法并传入参数x
   
   def str(x):
       t = type(x)
       if hasattr(t, '__str'):
           return t.__str__(x)
       else:
           return repr(x)	# 找不到str方法就用repr方法
   ```

4. 面向对象: 一对对象传递消息, 消息就是彼此的成员对象/函数

5. 共享消息: 不同对象的相似行为, 可能共用. 

6. 接口就是共享函数集合. 例如很多对象都会实现\__repr__, \___str__ , 

7. 一些特殊函数, 一般有两个下划线

   1. \__init__  默认构造函数
   2.  \__repr__  在python交互器中输出string
   3. \__add__  add两个对象(radd 会把第二个数写在前面)
   4. `__bool__`  把对象转换成bool类型  one.\__bool__() 等价 bool(one)
   5. \__float__  转换成浮点

8. 模块化设计:  实现关注点分离!

9. sorted(iterable, key=None, reverse=False) 按照key来升序排列. key 函数接受一个参数, 用于和iterable元素进行比较







#### 链表

1. Link(3, Link(4, Link(5, Link.empty))) or Link(3, Link(4, Link(5)))  (省略LinK.empty)

2. ```python
   class Link:
       empty = ()	# 空sequense
       def __init__(self, first, rest=empty):
           # rest 为空节点 or Link类型的对象
           assert rest is Link.empty or isinstance(rest, Link)
           self.first = first
           self.rest = rest
           
       # property 成员, 
       @property	# 使用s.second就行, 不用输入 s.second() 函数括号
    	def second(self):
           return self.rest.first
       
       @second.setter	# s.second = 6 可以直接赋值
       sef second(self, value):
           self.rest.first = value
   ```



#### 树

1. 两种描述方法

   1. 递归描述: 从根生长,不断向下. 树有root label + branches. 每个branch是树
   2. 相关描述: 树的每个位置叫做节点, 有自己label, 知道父亲/孩子的node位置

2. ```python
   # 法一
   class Tree:
       def __init__(self, label, branches=[]):
           self.label = label
           for b in branches:
               assert isinstance(b, Tree)
           self.branches = list(branches)
       # 定义对象后可以输出的内容
       def __repr__(self):
           if self.branches:
               branch_str = ', ' + repr(self.branches)
           else:
               branch_str = ''
           return 'Tree({0}{1})'.format(repr(self.label), branch_str)
       def __str__(self):	# 调用print(t)输出
           # join 把字符串/数组/元祖 的元素 按照指定字符连接起来
           return '\n'.join(self.indented())
       def indented(self):		# indent 缩进, indented adj 锯齿状的
           lines = []
           # 递归基: 叶子无branches, return [str(self.label)]
           for b in self.branches:
               for line in b.indented():	# 递归调用
                   lines.append('  ' + line)	# 把所有叶子拼成一行, 一个str, 是list的一个元素．　前面要加上空格
           return [str(self.label)] + lines	# 自己 +　叶子
       def is_leaf(self):
           return not self.branches
           
   t = Tree(3, [Tree(4), Tree(5)])
           
   # 法二
   def tree(label, branches=[]):
       for b in branches:
           assert is_tree(b)
       # 把树看成数组, 节点依次排列
       return [label] + list[branches]
   def label(tree):
       return tree[0]
   def branches(tree):
       return tree[1:]
   
   # 用法一定义树
   def fib_tree(n):
       if n == 0 or n == 1:
           return Tree(n)
       left = fib_tree(n-2)
       right = fib_tree(n-1)
       fib_n = left.label + right.label
       return Tree(fib_n, [left, right])
   ```



#### 递归

1. 不断调用自己，注意递归基

2. ```python
   def fib(n):
       if n == 0 or n == 1:
           return n
       else:
           return fib(n-2) + fib(n-1)
   
   def count(f):
       def counted(n):
           counted.call_count += 1
           return f(n)
       counted.call_count = 0
       return counted	# 输入函数f, 返回计数函数
   
   fib = count(fib)	# 覆盖fib, 每次调用, call_count++
   fib(5)	#5
   fib.call_count	# 15
   ```

3. memoization: 把已经算过的中间结果存储起来

4. ```python
   def memo(f):
       cache = {}	# 字典存 {n, f(n)}
       def memoized(n):
           if n not in cache:
               cache[n] = f(n)
           return cache[n]
       return memoized	# 行为等价于f(f是存函数)
   
   fib = memo(fib)	# 必须同名覆盖!
   ```

5. jupyter 中%%time ... 可以计时



#### 其他

1. ```python
   from functools import reduce
   l = [2,3,4,5]
   mul = lambda x, y: x*y
   out = reduce(mul, l)	# 24
   
   # 翻转list
   l = [2,3,4,5]
   l[::-1]
   l = list(reversed(l))
   l.reverse()
   # reverse() 只能用于list 翻转
   # reversed() 可用于iterable, 返回迭代器, 用list, next来输出
   ```

2. 

