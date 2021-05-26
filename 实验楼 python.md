

### 学习方法

1. 做实验, 写代码. 只学理论是不够的!
2. 完成挑战, 好好思考. 这能陷入一种心流过程, 很快乐

3. 做好笔记:学习编程不可能一次性就记住所有知识, 正确做法: 完成一个章节后, 写好笔记, 或者画好思维导图, 方便之后查阅.

   好处:

   1. 记得更牢固, 用自己的语言表达后, 记得更加牢固了.
   2. 方便查阅: 以前直接复制粘贴, 但是要百度很多次才记住, 但是能有自己备忘录后, 能大大减少搜索时间
   3. 求值利器; 善于总结, 有很多学习笔记, 能证明自己.

4. 还可以和小伙伴一起学习, 分享自己的代码和作品, 相互鼓励



### 认识linux

注意不要强行记忆, 自己操作几次就记住了, 做好笔记,  之后随时翻阅

常用命令

```sh
cd -   		# 可以回到上次所在的目录, 类似后退
tree -L 1  	# 可以使用树的方法列出所有文件, level 1级
mkdir -p one/weo/three 	# 一次性创建多个目录, 忽略已存在的error
cp -r dir1 one/two		# cp -r recursive 递归, 直接复制目录
rm -r dir1 				# 递归删除整个文件夹
mv src dir1				# 移动文件, 或者重命名
ll 					# 列出所有文件的详细信息
```

对于很多命令, 用  man cat 或者 cat --help 来查找

最好方法: 去百度命令的用法, 然后亲自linux上实践!



下图为linux系统目录的每个子目录作用

![img](https://doc.shiyanlou.com/courses/uid8504-20190517-1558079206858)

世界变化的很快, 但是当世界需要我们学习什么的时候, 我们可以主动去学习, 并且学的很好. 那么焦虑, 担心这些负面情绪都不存在, 我们会更加快乐, 也比很多人走的更远! 要亲自动手去做!



### 认识python

一般两种: 

1. 交互式命令执行, 用于调试代码
2. 程序文件执行, 用于完整开发

```python
print('你好') 	# 单引号变成字符串
print('''
	你好
	我是
''')			# 打印多行, 用'''  三个引号'''
```
数据类型
```python
# int
# float
1.1 + 2.2 = > 3.0000003		# 先转成二进制再处理, 有精度缺失
# bool
True, False 	# 注意大小写
None 		# None, 一个特殊值, 注意不是0, 表示空
```

input函数
```python
age = input('My age is :') 	# age是一个string
```

字符串

```python
#如果字符串中还有单引号' , 需要转义 \' 
# 使用下标获取字符串索引, string[-1] 表示最后一个
# format() 用于格式化字符串, 常用于插入数据, 数字格式化
print('你叫{}，今年{}岁了'.format(name,age))  # 用{} 占位, 插入数据
print('{:.2f}'.format(3.14159)) 	# 3.14  , 注意 {:.}
```

注释很重要!

1. 理清楚思路
2. 提高程序的可维护性
3. 如果暂时不用代码, 先注释而不是删除

运算符

```python
/  # 两数字除法, 返回小数点
//  # 返回商的整数部分, 向下取整数, 3.9 -> 3
**  # 2 ** 3 = 8, 取幂运算
# 逻辑 运算符, 尽量用于 True, False, 1 , 0 
and, or, not
```



### python 流程控制

条件判断

```python
a = int(input("Please enter a number: "))  # 转换成int(input())
if a > 10:   # 注意冒号, 缩进
    print('a > 10')
elif a == 10:
    print('a == 10')
elif a == 9:
    pass 	# 没想好怎么写, 用pass跳过这里
else:
    print('a < 10')
```

循环控制

```python
for item in iterable:
    do_sth
for i in range(1, 10):	# [1, 10), 1到9
    print(i)
# break 停止当前循环
# continue 跳过当前轮次循环, 继续下一次循环
```

```python
for i in range(101):
	if i % 7 == 0:  # ?7?
		continue
	if i % 10 == 7: # ????7
		continue
	if i // 10 == 7: # ????7
		continue	
	print(i)
```





### 图片转字符



```shell
sudo pip3 install --upgrade pip  # 更新pip版本
sudo pip3 install pillow
wget http://labfile.oss.aliyuncs.com/courses/370/ascii.py
```

灰度值, 0= 黑, 255 = 白

RGB , 使用红绿蓝, 对应, 可以转换成灰度值  gray ＝ 0.2126 * r + 0.7152 * g + 0.0722 * b

创建一个字符列表, 灰度值小(暗淡)的用列表开头的符号, 亮的用列表末尾的符号

```python
# /home/yanyue/PycharmProjects/crawler/week1.1/ascii.py
from PIL import Image
import argparse

# 创建parser
parser = argparse.ArgumentParser()  # 注意这里一定有括号!

# 定义输入输出文件, 输出字符串的宽和高
parser.add_argument('file')
parser.add_argument('-o', '--output')
parser.add_argument('--width', type=int, default=80)
parser.add_argument('--height', type=int, default=80)

# 获取参数
args = parser.parse_args()

IMG     = args.file
OUTPUT  = args.output
WIDTH   = args.width
HEIGHT  = args.height

# 默认使用的字符集合
ascii_char = list("$@B%8&WM#*oahkbdpqwmZO0QLCJUYXzcvunxrjft/\|()1{}[]?-_+~<>i!lI;:,\"^`'. ")

# RGB变成字符串, alpha = 0, 表示图片位置空白
def get_char(r, g, b, alpha = 256):
    if alpha == 0:
        return ' '
    length = len(ascii_char)
    gray = int(0.2126 * r + 0.7152 * g + 0.0722 * b)
    unit = (256.0 + 1) / length     # 灰度值范围0-255, 字符串范围0-70
    idx = int(gray / unit)
    return ascii_char[idx]

# 表示如果本文件被当成python模块而import时候, 这部分不会执行
if __name__ == '__main__':
    # 使用pil打开文件
    im = Image.open(IMG)
    # im.resize 调整宽度, 高度
    im = im.resize((WIDTH,HEIGHT), Image.NEAREST)   # nearest低质量
    # 遍历每个像素, 获取字符
    txt = ""
    for i in range(WIDTH):
        for j in range(HEIGHT):
            txt += get_char(*im.getpixel((j,i)))	# * getpixel 返回rgb参数
        txt += '\n'
    # 拼接成txt
    # 打印txt
    print(txt)
    # 如果有输出文件, 输出到文件
    if OUTPUT:
        with open(OUTPUT, 'w') as f:
            f.write(txt)
    else:
        with open("output.txt", 'w') as f:
            f.write(txt)
```

