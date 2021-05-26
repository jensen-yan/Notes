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

8. for x in 列表

9. print

10. range([start], stop [, step = 1]), 不包括stop

11. ```python
    def print(self, *args, sep=' ', end='\n', file=None): 
    # 默认end为'\n' 换行
    ```

12. break是跳出当前循环, continue是进入下一次循环



#### 列表

1. 可以使用append, insert 来操作【1,2,3】

2. 使用pop, append(没有push) 可以当做栈来用, pop(0) 可以弹出栈底元素

3. ```python
   squares = [x**2 for x in range(10)]
   squares = list(map(lambda x: x**2, range(10)))
   ```



#### 元组

1. tuple由逗号分隔   `a=12, 2, 1`  , 是不可变的类型, 不能添加删除  `a=12,` 表示只有一个值
2. 使用`type(a), len(a)` 



#### 集合

1. set是无序不重复元素的集合, 可以关系测试, 消除重复元素 `a={23,"se", 2}  a=set()`, 注意`a={}` 表示创建空字典!
2. `a = set('abracadabra')` 可以自动去重复 ` a ={'a', 'r', 'b', 'c', 'd'} `
3. 操作: ` a-b  a| b  a &b  a^b`



#### 字典

1. 是无序的键值对 `da = {12:3, 23:"a"}`
2. dict(ar) 把元组转换为字典
3. `for x, y in data.items():` 来遍历字典
4. 往字典中的元素添加数据，我们首先要判断这个元素是否存在，不存在则创建一个默认值。如果在循环里执行这个操作，每次迭代都需要判断一次，降低程序性能。我们可以使用 dict.setdefault(key, default) 更有效率的完成这个事情
5. 试图索引一个不存在的键将会抛出一个 keyError 错误。我们可以使用 dict.get(key, default) 来索引键，如果键不存在，那么返回指定的 default 值。
6. 在遍历列表（或任何序列类型）的同时获得元素索引值，你可以使用 enumerate()。`for i, j in enumerate(['a', 'b', 'c']):`    需要同时遍历两个序列类型，你可以使用 `zip()` 函数。`for x, y in zip(arr1, arr2)`

