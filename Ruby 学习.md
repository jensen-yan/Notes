### Ruby 学习

#### 概论

1. 分号, 换行符为语句结束;  + - \ 表示语句后续
2. ''#'' 单行注释 , =begin ...   =end 多行注释
3. 有IRb 控制台程序, 逐行执行, 命令行输入irb即可,  使用`irb --simple-prompt` 让结果更易读, exit 退出
4. 脚本编程(.rb文件), 使用解释器`ruby` 来运行
5. puts 输出数组【1,2,3】 会分成三行输出, 



#### 方法

1. 方法名需要小写开头(大写开头会看作常量), 方法要找调用之前定义

2. 局部变量: 小写字母/ 下划线_ 开头,

   ```ruby
   def say_hello(name='World')
    puts "Hello, #{name}!"			# 使用#{}来访问任何变量或常量的值
   end
   
   say_hello         # => "Hello, World!"
   say_hello("John") # => "Hello, John!"
   ```

3. 可变参数`def sample(*test)`

4. 返回值: 默认返回最后一个语句的值;  或者 return a, b



#### 全局变量

1. 以$ 开头, 默认为nil的初始值, 用 -w  会报警告, 由于改变全局状态, 不建议使用
2. 使用全局变量也要加$,  例如输出时候用 `puts 'hello, #{$num}'`  否则错误很难找啊!

#### 常量

1. 以大写字母开头, 定义在类/模块内的可以在内部访问;  定义在外面的可以全局访问
2. 常量不能定义在方法内
3. 引用一个未初始化常量有错误, 对已经初始化的常量赋值也有警告

#### 伪变量

1. 特殊的量如下

   - `self`: 当前方法的接收器对象。
   - `true`: 代表 true 的值。
   - `false`: 代表 false 的值。
   - `nil`: 代表 undefined 的值。
   - `__FILE__`: 当前源文件的名称。
   - `__LINE__`: 当前行在源文件中的编号。

   

#### 字符串

1. String 是一个提供的类, 有庞大的方法集

2. 创建: 使用 '  or  ''  来做(多用'',  有时候字符串内部有 ' )

3. 转义字符: ` \b 退格, \r 回车符, \s 空格, \t tab, \a 蜂鸣警告`

4.  常用方法

| 方法调用方式                     | 返回值            | 描述                                                         |
| -------------------------------- | ----------------- | ------------------------------------------------------------ |
| `str.length`                     | `Integer`         | 字符串的长度，空字符串返回 0                                 |
| `str.include?(other_str)`        | `True` 或 `False` | 字符串包含，传入的参数为另一个字符串，如果该字符串中包含该子串，则返回`True`，否则返回`False` |
| `str.insert(index, other_str)`   | `new_str`         | 字符串插入，参数`index`是待插入的下标，`other_str`是另一个字符串，返回的是插入后新的字符串，下标从 0 开始计算 |
| `str.split(pattern=$;, [limit])` | `Array`           | 字符串分割，将字符串按照`pattern`进行分割，默认分割符为空格，返回值是一个包含若干字符串的数组 |
| `str.gsub(pattern, replacement)` | `new_str`         | 字符串替换，将字符串按照`pattern`匹配的字符更换为`replacement`，返回替换后的字符串 |
| `str.replace(other_str)`         | `other_str`       | 字符串整体替换，将字符串整体替换成新的字符串                 |
| `str.delete([other_str]+)`       | `new_str`         | 字符串删除，传入参数`[other_str]+`可包含多个字符，该方法匹配到`str`中的所有字符并删除，返回新的字符串 |
| `str.strip`                      | `new_str`         | 清除空格，清除掉`str`中字符串前后的所有空格，换行符，回车符。不包含字符间的空格，返回新的字符串 |
| `str.reverse`                    | `reverse_str`     | 字符串翻转，将字符串顺序翻转，返回翻转后的字符串             |
| `str.to_i`                       | `Integer`         | 字符串转换为数字， 如果字符串以数字开头，则转换为开头数字的整型值，如果字符串不以数字开头，则返回 0 |
| `str.chomp`                      | `new_str`         | 去掉字符串末尾的`\n`或`\r`                                   |
| `str.chop`                       | `new_str`         | 去掉字符串末尾的最后一个字符,不管是`\n`，`\r`还是普通字符    |
| `str.downcase`                   | `new_str`         | 将字符串转换为全小写                                         |
| `str.upcase`                     | `new_str`         | 将字符串转换为全大写                                         |



#### 数组

1. 数组Array可以存储 String, Integer, Fixnum, Hash, Symbol等对象, 也可以是其他Array(成为多维数组), 添加元素时候, 会自动增长.

2. 创建: 

   1. new方法: arr = Array.new,   new(5) 表示长为5的空Nil数组,   arr = Array.new(5, 'coder')
   2. Array.\[]( 1, 2, 3) 
   3. Array[1, 2, 3]
   4. [1, 2, 3]  # 最常用

3. 访问数组元素, 用index索引, 从0开始, -1为最后一个.  eg: arr\[2,4](表示范围),   arr[1..4], arr.at(0)

   1. 还可以用 arr.take(3) 访问前3个,  drop(3) 访问索引3之后的所有元素 
   2. (1..5) 这就表示Range了,  (1..5).to_a  把Range 转换成 Array

4. 常用方法 

   | 方法调用方式                   | 返回值            | 描述                                                         |
   | ------------------------------ | ----------------- | ------------------------------------------------------------ |
   | `arr.empty?`                   | `true` 或 `false` | 判段数组是否为空                                             |
   | `arr.push(element)`            | `new_array`       | 在数组的最后加入元素`element`，返回加入元素后的新数组        |
   | `arr << 6`                     | `new_array`       | 同上                                                         |
   | `arr.insert (index, elements)` | `new_array`       | 在指定位置`index`塞进元素`elements`，可以塞多个元素 `arr.insert (3, 'a', 'b', 'c')` |
   | `arr.delete(element)`          | `element`         | 删除数组中所有为`element`的元素                              |
   | `arr.compact`                  | `new_array`       | 删除数组中所有空元素` nil`，返回删除空元素后的新数组         |
   | `arr.uniq`                     | `new_array`       | 清除数组中的重复元素，返回去除重复后的新数组                 |
   | `arr.reverse`                  | `new_array`       | 翻转`arr`，返回翻转后的新数组                                |
   | `arr.clear`                    | `[ ]`             | 删除数组中的所有元素，返回空数组                             |
   | `arr.count`                    | `Integer`         | 没有参数时，返回数组的大小，带有参数时，返回数组中与参数相同元素的个数 |
   | `arr.includes?(element)`       | `true` 或 `false` | 判断数组`arr`中是否包含元素`element`                         |
   | `arr.sort`                     | `new_array`       | 将数组按照首字母进行排序，也可定制排序规则                   |
   | `arr.sample`                   | `element`         | 从数组中随机取样，带参数取样个数，则可取多个样本             |
   | `arr.flatten`                  | `new_array`       | 将多维数组转换成一维数组                                     |
   | `arr.join(',')`                | `String`          | 将数组使用连接符`,`连接成一个字符串                          |



#### Hash 哈希

1. 使用 key -> value 的键值对的集合, 索引为任何对象类型的任意键来完成, 不是整数索引, 其它和数组类似. 注意不会维持顺序, 只负责两个对象的配对. 若通过不存在的键访问哈希, 返回nil

2. 创建:

   1. new:  hash = Hash.new(0)

   2. 如下方法

      ```ruby
      language = {}
      language['Ruby'] = 'Wonderful'
      language['Python'] = 'Excellent'
      # 或者如下
      language = {"Ruby" => "Wonderful", "Python" => "Excellent"}  
      language = { Ruby: "Wonderful", Python: "Excellent" }  # 更推荐, 
      # 后者使用 :,  key 为 :Ruby 是一个符号Symbol, 更高效
      ```

3. 访问: 用language.keys 查看所有的键, language["Ruby"]直接访问

4. 常用方法 

   | 方法调用方式            | 返回值            | 描述                                                         |
   | ----------------------- | ----------------- | ------------------------------------------------------------ |
   | `hsh == other_hash`     | `true` 或 `false` | 判断两个哈希是否相等。键相等，值相等，键值对应关系均相等才返回`true` |
   | `hsh.clear`             | `{}`              | 清空当前哈希                                                 |
   | `hsh.delete(key)`       | `value`           | 从哈希中删除匹配`key`的键值对，并返回对应的值`value`         |
   | `hsh.has_key?(key)`     | `true` 或 `false` | 判断哈希中是否包含键`key`                                    |
   | `hsh.has_value?(value)` | `true` 或 `false` | 判断哈希中是否包含值`value`                                  |
   | `hsh.to_s`              | `String`          | 将哈希的内容转换为字符串输出                                 |
   | `hsh.invert`            | `new_hsh`         | 将哈希的键值对颠倒，键变值，值变键，构成新的哈希返回         |
   | `hsh.key(value)`        | `key`             | 返回给定值对应的键。如果找不到该值，则返回0                  |
   | `hsh.length`            | `Integer`         | 返回哈希的大小                                               |
   | `hsh.merge(other_hash)` | `new_hsh`         | 将`hsh`和`other_hash`合并成一个新的哈希。重复键的项值采用`other_hash`。 |
   | `hsh.store(key, value)` | `value`           | 在哈希中加入新的键值对                                       |
   | `hsh.to_a`              | `Array`           | 将哈希转换为数组，格式为： `[[ key1, value1 ], [ key2, value2 ]]` |

   

#### 块

1. 一段代码, 传入参数时候可以看作一个方法调用, 块是一个强大的模块. 可以用块实现回掉, 并实现迭代器.

2. 块是括号{ } (只有一行代码),  do 和 end 之间(多行代码)的代码块. 放在需要调用块的代码行的末尾.

3. 使用yield声明, 可以多次调用这个块. yield看作方法调用, 调用这个块外的一段代码

   ```ruby
   def call_back  # 这就是一个迭代器
       puts "Start of method"
       yield
       yield
       puts "End of method"
   end
   call_back {puts "In the block"}
   ```

   此外还可以给yield传递参数 `yield("hello", 99)`,   `call_block {|str, num| ... }`



#### 迭代器

1. 重复执行多次相同事情, 用迭代器.

2. Iterator是一个简单的能接受代码块的方法(例如each方法), 若方法中包含yield调用, 肯定是迭代器.

3. 与块的传递关系(相互传递):  块被当做一个特殊参数传递给迭代器方法; 迭代器方法内部使用yield调用代码块时候可以把参数值传入块.

4. ruby的容器对象(例如Array, Hash, Range)有两个简单的迭代器:

   1. each: 最简单, 对容器每个元素调用块
   2. collect: 把对象元素传递给块, 块处理后返回一个包含处理结果的新数组Array

5. each(获取数组每个元素),   each_with_index(获取数组元素和下标)

   ```ruby
   arr = [1, 2, 3, 4, 5]
   arr.each { |num| puts num }
    # num 是一个局部变量，each迭代器每一次迭代将数组中的一个元素赋值给 num
   
   languages = ['Ruby', 'Javascript', 'Java']
   languages.each_with_index do |lang, index|	
       puts "#{index}, I love #{lang}!"
   end
   ```

6. collect(获取数组元素, 处理后返回新数组)

   ```ruby
   a = [1,2,3,4,5]
   b = a.collect{ |x| x * 2 }
   ```



#### 循环

1. 有四种常见循环结构 while, do...while, until,  for

2. while 语句

   ```ruby
   while conditional [do / :]
       code
   end
   #  do 或 : 可以不写, 如果在一行内写while需要do : 来隔开条件和code
   ```

3. do-while 语句(相比while, 至少执行一次)

   ```ruby
   begin
       code
   end while conditional
   # 或者
   code while conditonal
   ```

4. for语句

   ```ruby
   for variable [, variable ...] in expression [do]
       code
   end
   # 对expression(例如一个数组)中每个元素分别执行一次code
   ```

5. until语句, 和while区别: conditional为false进入循环

   ```ruby
   i = 1
   num = 5
   until i > num  do   # 等价于 while i <= num
     puts("我们爬了#{i}层楼" )
     i = i + 1
   end
   ```



#### 条件判断

1. if else 语句

   ```ruby
   if condition1 [then] # 多条件用and , or 连接
     code1
   [elsif condition2 [then]  # 使用elsif , 不是elif, else id
     code2]...
   [else
     code3]
   end  # 一定有end结尾!
   ```

2. unless 语句, (if为假), 不推荐使用.

3. case语句, 类比 switch case 语句

   ```ruby
   price = 85
   case price
   when 0..10
     puts "便宜"
   when 11..50
     puts "普通"
   else
     puts "贵"
   end
   ```

4. 其他

   1. break: 终止最内层循环, 配合if;
   2. next: 跳到循环下次迭代
   3. redo: 重新开始最内部循环的该次迭代, 重新执行一遍本次迭代

   

#### 类

1. class Person,  类名首字母要大写. 类就是一段代码, 和函数, 对象没有本质区别.

   ```ruby
   3.times do  # 用3.times 来重复执行3次
     class A	# 其实只定义了一个类A, 但是使用了3次
       puts "Hello"
     end
   end
   ```

2. 可以只任何时候打开一个已经存在的类, 并添加新的方法, 包括String, Array这样的标准类也可以添加自己的方法.(不同于其他语言, 可能导致在不同时间在同一类中引入一个同样的方法, 应该混乱, 成为猴子补丁)

3. 类也是一种对象, Class有自己的类为Class. 限制更少, 可以像对象那样在运行时, 定义动态的类, 允许编辑类.

4. 类的方法:

   1. 实例方法,  更常用

      ```ruby
      class Test  
          def hello   # def self.hello or  def Test.hello,  三种方法均可
              puts "hello"
          end
      end
      
      Test.new.hello  # 先new定义实例再调用
      ```

   2. 类方法

      ```ruby
      Test = Class.new	# 利用一个影子类, 为Test对象创造一个单例方法
      def Test.hello
          puts "Hello"
      end
      Test.hello
      ```

      

5. 子类继承父类 `class child < father`, 只支持单继承(多重继承使用Mix-In), 若子类和父类定义同名方法, 子类方法会覆盖父类方法

   ```ruby
   class Father
     def initialize(name)  # 构造函数
       @name = name		# 类的实例变量 @name
     end
     def says
       puts "I am father"
     end
   end
   class Son < Father
     def initialize(name) 	# 覆盖了父类的
       @name = "son_#{name}"
     end
     def says
       puts "I am son. name: #{@name}"
     end
   end
   son = Son.new("Jack") 	# 使用Son.new  去创建新的对象
   son.says       #=> I am son. name: son_Jack
   ```

6. 类变量:  @@开头, 必须初始化后才能调用, 作用于类的所有范围, 所有实例对象共享, 包括子类和实例, 通过Protected声明.

7. 实例变量: @开头, 可以跨实例/对象 中的方法使用, 在不同对象之间可以改变. 每个实例对象都有自己独有的实例变量.  (区别类实例变量)




#### 模块

1. 模块(Module)相当于类的扩充, 是方法和常量的集合.  两个好处:

   1. 提供了一个命名空间, 防止命名冲突
   2. 能实现Mix-in 功能

2. 模块和类的不同:

   1. 模块不能被实例化
   2. 模块不能被继承, 无子模块

3. 模块的方法:

   1. 模块方法: 和类方法类似, 暴露出来可被外部使用

   2. 实例方法: 不能被外部引用, 需要在类中引用(include / extend)模块, 作为类的方法使用

      ```ruby
      module TestModule
        def module_function_say_hello		# 模块方法
          puts "Hello! I'm Module Function!"
        end
        def instance_function_say_hello	# 实例方法
          puts "hello! I'm instance function!"
        end
        # 将方法定义为模块方法, 外部随意使用
        module_function :module_function_say_hello
      end
      # 调用模块方法
      puts TestModule.module_function_say_hello
      # 使用模块名调用模块的实例方法，产生 NoMethodError 错误
      TestModule.instance_function_say_hello
      
      class TestClass
        # 在类 TestClass 中引入模块 TestModule
        include TestModule
      end
      test_object = TestClass.new
      # 调用模块的实例方法（类的成员方法）
      puts test_object.instance_function_say_hello	
      # 使用对象调用模块方法，产生 非法访问 错误
      test_object.module_function_say_hello
      
      class TestClass2
        extend TestModule		# 使用extend, 变成类的类方法引入
      end
      TestClass2.instance_function_say_hello
       #=> hello! I'm instance function!
      TestClass2.module_function_say_hello
       #=> NoMethodError: private method `module_function_say_hello' called for TestClass2:Class
      ```

   3. 注: 模块方法作为模块的公有方法被外部访问, 但被引入到类后, 变成了Private, 外部不可用;

   4. 注: 实例方法, 由于模块不能被实例化, 通过类的引入(include)来把模块中所有实例方法变成这个类的**实例方法**.  extend 来变成类的**类方法**访问

4. require语句: 类似include, import, 若第三方想使用已定义的模块, 用require加载模块文件



#### Mix-In

1. 实现继承, 一种受限制的多重继承, 可以对某些类进行"点缀".

2. 多重继承的3个问题
   1. 结构复杂化, 有多个父类, 不是线性关系
   2. 优先顺序模糊
   3. 功能冲突: 不同父类有相同方法的冲突
   
3. 在类中使用include 模块, 方法查找顺序:
   1. 优先使用类自己的同名方法
   2. 类中有多个模块, 优先使用最后一个模块
      1. 多级嵌套(include), 查找还是线性
      2. 相同模块被include了两次, 第二次之后会被忽略
   
4.  ```ruby
    module Mod1
      def meth
        puts "这是Mod1"
      end 
    end
    module Mod2
      def meth
        puts "这是Mod2"
      end 
    end
    module Mod3
      include Mod2
      include Mod1
      include Mod2
    end
    class Class1
      include Mod1
      include Mod3
    end
    object_class = Class1.new
    object_class.meth      #=> 这是Mod1
    p Class1.ancestors
     #=> [Class1, Mod3, Mod1, Mod2, Object, Kernel, BasicObject]
    ```



#### 输入输出

1.  gets: 获取标准屏幕的用户输入, 最后会有\n 字符, 可用gets.chomp去除

   1. 也可以传参调用`gets(sep, limit)` , sep 是分隔符, limit 是输入上限字符, 若超过, 则分为多段

2. ARGV 从命令行接受参数, ARGV[0] = Educoder  (输入ruby test.rb Educoder)

3. 输出: 

   1. 常见的 puts , p 方法

   2. STDOUT 

      ```ruby
      $stdout << "hello " << "world\n"
      ```

   3. printf 方法, 同C语言, %d, %f, %x, %o, 常见的格式序列如下：

      格式域	作用
      b	作为二进制输出
      c	作为字符表示输出（ASCII码表）
      d	作为整数输出
      f	作为浮点数输出
      o	作为八进制数输出
      s	作为字符串输出
   u	作为无符号小数输出
      x	作为十六进制数输出，a-f为小写字母
   X	作为十六进制数输出，A-F为大写字母
      \n	换行输出
   
   4. 
   
      ```ruby
      # `#' 号在输出八进制数时使其前面带上八进制数标志 `0`
      # `+' 号改变负数的输出格式
      printf("%o", 123)   #=> "173"
      printf("%#o", 123)  #=> "0173"
      printf("%+o", -123) #=> "-173"
      ```
   
   5. putc(obj), 输出一个字符
   
      1. 若obj为数字, 查ascii, 转换为字符输出
      2. 若obj为字符串, 只输出第一个字符



#### 文件操作

1. 创建  `File.new(filename, mode="r" [, opt])`  filename 为需要操作的文件名（可带路径）, mode 有 "r r+ w w+ a a+ b t",   mode默认为"r 只读"

   1. ```ruby
      File.open("filename", "mode") do |file|
          # ... 处理文件
      end		# 之后自动关闭文件
      
      f = File.new("testfile")		# 也可用new
      ```

2. 文件读写

   1. 简单IO读写也适用于文件对象, file.gets  读取一行

   2.  文件独有的: file.read([length]) 读取指定个数字符, 若length被忽略会读取到EOF

   3. 还有 readline(读取一行包括\n, 并放到下一行等待继续读),  readlines(读取整个文件, 按照行号返回一个数组)

   4. ```ruby
      # testfile 文件保持与上述一致
      f = File.new("testfile", "a+")
      f.write("This is new line!")		# 文件写入
      # 字符串写入到末尾, 注意\n 可能要添加! 
      ```



#### 目录操作

1. File 处理文件, Dir 类 处理目录 `Dir.new(dir_name)`  创建目录对象

2. 基本操作

   1.  `Dir.chdir("/usr/bin")`  切换目录

   2. `puts Dir.pwd`  查看当前目录

   3. `puts Dir.entries("./").join(" ") `  entries 获取指定目录的文件和目录列表, 返回 数组,   eg: 输出`".. file.rb . testfile"`

   4. ```ruby
      Dir.foreach("/usr/bin") do |entry|
      	puts entry
      end		# 也可以遍历
      ```

   5.  Dir.mkdir( "mynewdir", 755 ) 来给新创建的目录增添权限掩码为 755 的权限。

   6. Tips：掩码 755 设置所有者（owner）、所属组（group）、每个人（world [anyone]）的权限为 rwxr-xr-x，其中

      读取: r = read = 4
      写入: w = write = 2
      执行: x = execute = 1

   7.  ```ruby
      require 'tmpdir'
      tempfilename = File.join(Dir.tmpdir, "test_temp_file")
       #=> tempfilename 的值为 "/tmp/test_temp_file", 创建临时文件
      tempfile = File.new(tempfilename, "w")
       ```

   8. Dir.delete("testdir") 删除目录,  unlink, rmdir 同理,  注意目录非空会报错

