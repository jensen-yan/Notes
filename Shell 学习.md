## Shell 学习

```bash
#!/bin/bash
# 一般第一行加上 #!/bin/bash
echo "Hello World !"


chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```

#### 变量

1. 定义变量: name="yanyue",  注意等号前后不能有空格!
2. 使用变量:  echo $my_name / ${my_name},  加上$
3. 只读变量: readonly $name
4. 删除变量: unset $name
5. 三种变量
   1. 局部变量: 只在当前shell有效
   2. 环境变量: 所有程序都能访问, shell脚本也能定义
   3. shell变量: 由shell程序设置的, 包括上面两种