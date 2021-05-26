### sed

参考博客http://c.biancheng.net/view/994.html

linux shell sed命令: 小巧, 能对流数据来编辑, 不用存储在文件中, 用书数据的选取, 替换, 删除, 新增

sed [选项] '[动作]' 文件名

选项包括;

1. -n, 只输出修改的行
2. -e, 允许对数据进行多条sed操作
3. -f  脚本文件名, 从sed脚本读取操作
4. -r  正则表达
5. -i  sed修改直接修改文件内容

注意sed修改不会改变文件内容, 只是把结果显示到屏幕上, -i才修改文件 

动作包括:

1. a \: 追加, 在当前行后追加一行或多行, 其中多行用 \ 划分行
2. c \: 行替换, 用c后面的字符串替换原有数据行
3. i \: 当前行之前插入一行或多行
4. d: 删除指定行
5. p: 打印指令行
6. s: 字符串替换, 格式: 行范围s/旧字符/新字符/g

例子

```shell
sed -n '2p' README.md	# 输出第二行
sed '2,4d' README.md 	# 删除2到4行, 其中逗号表示范围, '2,$d'表示$删除到最后一行
sed '1s/sodor/yanyue/g' README.md	# 把第一行sodor替换成yanyue
sed '4s/^/#/g' student.txt		# ^表示行首, 换成# 注释第四行
sed -e 's/Liming//g; s/Gao//g' student.txt  # 把Liming, Gao替换成空, 多行命令用; 分隔, -e表示多个sed操作
```



### tee

参考https://blog.csdn.net/dazhi_100/article/details/45022253

可以使用tee命令把输出同时输出到屏幕和文件中

vasp > out.txt   只输出到文件

vasp | tee out.txt  同时输出

vasp | tee out.txt | less 	对屏幕的用less防止过多输出

注意只能把标准输入中读结果, 把结果写到标准输出和文件中

tee [选项]  [文件]

选项:

-a ,  --append 追加到文件后面

-i , ignore-interrupe 	忽略中断

-p, 对写入非管道行为排查错误

--output



### tar

-c 压缩 create, 默认打包, 使用z才压缩

-x 解压

-t 查看

其中c, x , t 只能存在一个

-z 使用gzip压缩

-j 使用bzip2 压缩

-v 压缩过程显示文件信息

-f 对于哪个压缩文档名, 一般放最后

-P 可以使用绝对路径压缩

--exclude FLIE 不打包FILE文件



### xargs

护花使者, 帮助其他命令来完成. execute arguments 的缩写

从标准输入读取内容, 传递给协助的命令, 并作为那个命令的参数来执行

和管道的关系

```shell
cat a.txt	# 显示a.txt 内容
echo a.txt | cat # 显示 a.txt 本身
echo a.txt | xargs cat # 显示内容
```

管道可以实现: 把前面的标准输出变成后面的标准输入

管道不能实现: 把前面的标准输出变成后面的**命令参数**

对于后者, 需要使用xargs 来完成传递参数的作用