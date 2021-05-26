## Makefile 学习

参考博客 http://www.ruanyifeng.com/blog/2015/02/make.html  阮一峰的博客

### 一. make 概念

```makefile
a.txt: b.txt c.txt
    cat b.txt c.txt > a.txt
```

make a.txt 制作a.txt文件

1. 确认依赖b.txt, c.txt 存在
2. 使用shell命令 cat



Makefile文件也可以写成makefile, 或者用 -f指定其他的文件名

```
make -f rule.txt
make --file=rule.txt
```

make 是根据指定shell命令进行构建的工具



### 二.Makefile文件的格式

有一系列规则构成

```
<target>: <prerequisites>   # 前置条件
[tab]  <commands>
```

一个target构成一条规则, target一般是文件名, 亦可以多个文件名, 还可以是一个操作的名字(伪目标  clean)

```makefile
.PHONY: clean 	# 声明成伪目标, 即便有clean文件也可以用
clean:
	rm *.o temp
```

如果直接make, 默认执行第一个目标

可以用.DEFAULT_GOAL = app 来默认make目标

#### 前置条件

前置文件如果不存在, 或者有更新, 目标就要重新构建. 否则不构建

如果要生成多个文件

```makefile
source.txt: file1 file2 file3	# 无命令
$make source.txt   # 直接生成file1, 2, 3 文件
```

#### 命令

表示如何更新目标文件, 有多行shell命令构成.

每个命令之前必须有tab键, 或者用内置变量.RECIPEPREFIX声明。

```makefile
# 每行命令都在一个单独shell中指令, 没有继承关系, 在不同进程中执行
var-lost:
	export foo=bar
	echo "foo=[$$foo]" 	# 无法获得foo的值
var-kept:
	export foo=bar; echo "foo=[$$foo]"   # 一行就可以
# 或者加上 \ 反斜杠, 或者用.ONESHELL:
.ONESHELL:
var-kept:
    export foo=bar; 
    echo "foo=[$$foo]"
```

### 三.Makefile文件语法

#### 回声

正常情况下, make会打印每条命令, 再执行, 包括中间的# 注释

在命令前面加上@ , 可以关闭回声

一般只在注释, 和纯粹显示的echo前面加上@

#### 通配符

wildcard通配符指定一组符合条件的文件名, 和bash一样, 有 *, ?, ...

#### 模式匹配

主要用%

例如当前目录有 f1.c f2.c文件

```makefile
%.o: %.c
# 等价于
f1.o: f1.c
f2.o: f2.c
```

使用% 可以把大量同类型的文件一次构建

#### 变量和赋值符

使用等号自定义变量, 调用时候, 变量放在$( ) 中

```makefile
txt = Hello World
test:
	@echo $(txt)
```

调用shell变量, 需要使用$$ 两个符号, 因为Makefile对$ 转义

```makefile
test:
	@echo $$HOME
```

如果让一个变量指向另一个变量 v1 = $(v2), 有问题:

如果v2是动态的, 那么v1在定义扩展(静态), 还是在运行时扩展(动态). 结果不同. 

为了解决问题, makefile定义四种赋值符号 = := ?= +=

```makefile
v1 = v2
# 执行时扩展, 允许递归
v1 := v2
# 定义时静态扩展
v1 ?= v2
# 只有在v1 是空的时候才设置值
v1 += v2
# 把v2 追加到v1后面
```

#### 内置变量

$(CC) 指向当前使用的编译器,    (MAKE) 指向当前使用的Make工具

ifneq ($(MAKECMDGOALS),clean).  \$(MAKECMDGOALS) 表示make的伪目标

#### 自动变量

1. $@ 指向当前目标; make foo 就指向foo

   ```makefile
   a.txt:
   	touch $@
   # 等价
   a.txt:
   	touch a.txt
   ```

2. $< 指代第一个前置条件

   ```makefile
   a.txt: b.txt c.txt
   	cp $< $@
   	# 等价
   	cp b.txt a.txt
   ```

3. $? 指代比目标更新的所有前置条件, 用空格分开

4. $^ 表示所有前置条件

5. $* 表示% 匹配的部分, 例如% 匹配f1.txt中的f1, 它就表示f1

6. \$(@D),  \$(@F) 分别指向目标的目录名和文件名, 例如$@ = src/input.c, \$(@D)表示src, \$(@F) 表示input.c

7. \$(<D) 和 \$(<F) 分别指向 \$< 的目录名和文件名。

   ```makefile
   dest/%.txt: src/%.txt
   	@[ -d dest] || mkdir dest
   	cp $< $@
   ```

   把src中的txt文件拷贝到dest中, 如果dest目录不存在, 先新建.

#### 判断和循环

```makefile
ifeq ($(CC), gcc)
	libs = $(libs_for_gcc)
else
	libs = $(normal_libs)
endif
```

```makefile
# for循环
LIST = one two three
all:
	for i in $(LIST); do \
		echo $$i; \
	done
# foreach
$(foreach VAR,LIST,CMD 1;CMD 2;)
$(foreach i, $(LIST), \
	echo $i)
```

#### 函数

常用的内置函数

1. shell 执行shell命令

   ```makefile
   srcFiles = $(shell echo src/{00..99}.txt)
   ```

2. wildcard 通配符, 替换bash的通配符

   ```makefile
   srcFiles = $(wildcard src/*.txt)
   srcFiles = $(src/%.txt)  # 等价
   ```

3. subst 文本替换 格式: $(subset from, to, text)

   ```makefile
   $(subset ee, EE, feet on the street )
   # 将字符串"feet on the street"替换成"fEEt on the strEEt"。
   
   # 空格替换成,
   cooma := ,
   empty := 
   space := $(empty) $(empty)  
   # 两个空变量中间一个空格
   foo := a b c
   bar := $(subset $(space), $(comma), $(foo))
   # bar = a,b,c
   ```

4. patsubst 用于模式匹配的替换, 格式: \$(patsubst pattern, replacement, text)

   ```makefile
   $(patsubst %.c, %.o, x.c.c bar.c)
   # 把 x.c.c bar.c 替换成 x.o.c bar.o
   ```

5. 替换后缀名,  格式: 变量名+冒号+后缀替换规则, 是patsubst简写

   ```makefile
   min: $(OUTPUT:.js=.min.js)
   # 把变量OUTPUT的后缀.js 变成.min.js
   ```



### 四. 实例

执行多个目标

```makefile
.PHONY cleanall cleanobj cleandiff
cleanall: cleanobj cleandiff
	rm program
cleanobj:
	rm *.o
cleandiff:
	rm *.diff
# make cleanall 删除所有
```

编译C语言项目

```makefile
edit : main.o kbd.o command.o display.o 
    cc -o edit main.o kbd.o command.o display.o

main.o : main.c defs.h
    cc -c main.c
kbd.o : kbd.c defs.h command.h
    cc -c kbd.c
command.o : command.c defs.h command.h
    cc -c command.c
display.o : display.c defs.h
    cc -c display.c

clean :
     rm edit main.o kbd.o command.o display.o

.PHONY: edit clean
```



