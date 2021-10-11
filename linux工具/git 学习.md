### git

#### 简介

```sh
git remote add origin git@github.com:jensen-yan/Demo.git # push 到网络端
git remote add # 远程添加 
origin # 远程名字设置成origin, 后面网址为对应的
git push origin master  # 把主分支push到origin远程名字上
# 在github上创建仓库, 默认主机为origin, 分支是master

```

git本地仓库分为: 工作区, 暂存区, 版本区

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid9805timestamp1548755776759.png)



``` shell
git add one.txt		# 提交到暂存区
git reset -- 	# 把暂存区所有修改删除
git diff		# 查看工作区, 已经被追踪文件(之前commit过的文件, 在版本区域的文件), 的修改内容
git diff --cached  # git add . 提交到暂存区了, 查看修改内容(暂存区和版本库的区别) 
git log 		# 查看版本区的提交记录
git reflog		# 可以查看删除错误的记录
```

常用的git log 参数

```shell
git log --oneline	# 一行显示一次提交
git log -n			# 显示最近n次
git log --author jensen-yan	# 根据author来看
git log [分支名] 	  # 看对应分支的
git log --graph		# 图的方式看
git log --reverse	# 反着看
```

提交

```shell
git branch -avv		# 常用命令, 显示如下内容, -a 显示所有分支, -vv 显示详细信息
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid310176labid9805timestamp1548755963010.png)

1. 当前分支是master, 版本号是5c041ad, 对应的远程分支是origin/master, 领先一个分支
2. HEAD 指向远程分支, 远程master只有一次commit

最好方法: 先修改工作区-- 提交到暂存区 -- git status 多看看状态 -- git commit 提交到版本区



#### 版本回退

如果发现某个文件提交到版本区, 但是有问题

可以修改文件, 再次添加到暂存区, 提交, push;

也可以撤销最近一次提交, 修改后再重新提交,push, 方法如下:

```shell
git reset --soft HEAD^	# 撤销最近一次提交, 修改还原到暂存区, HEAD^ 撤销一次提交, HEAD^^ 撤销两次提交
# 修改某个文件
git add .
git commit -m "commit file one.txt"	
git log		# 可以看到覆盖了上一次版本commit
```



#### 处理commit时间线分叉

```shell
git status 	# 版本回退后和远程仓库不同了
git push -f 	# 强制推送
```

如果发现之前版本回退有问题, 还想返回到最早操作, 不用再次版本回退了

```shell
git reflog	# 可以查看本地仓库的所有记录
git reset --hard HEAD@{2} 	# 回退到某个位置
git reset --hard HEAD@{1} 	# 改回去了
```



### git分支操作

#### 添加ssh授权

```shell
ssh-keygen
tree ~/.ssh		# 存放在这里的
# ~/.ssh/id_rsa.pub 添加到github中
git remote set-url origin git@github....	# 必须这样的才能免密登录
```

#### 设置别名

```shell
git config --global alias.[别名] [原来命令]
git config -l 	# 查看已有的
cat ~/.gitconfig	# 也可以查看所有配置
# 我设置了3个别名
br -> branch -avv
st -> status
com -> commit -m
ch -> checkout
```

#### 分支管理

```shell
git fetch 	# 更新远程分支
git pull	# 拉取远程分支到本地
git rebase origin/master	# 让本地分支 等同于远程主分支, 和git pull 等价
```

分支开发很重要, 一般在自己分支修改, 然后向master提交PR(pull request), 然后从master pull过来

```shell
git branch dev
git checkout dev 	# 切换到dev
git checkout -b dev 	# 创建并切换到dev
git push origin [本地分支]:[远程分支]
```

希望本地分支也能直接git push提交, 能像主分支一样追踪远程分支

```shell
git branch -u [主机名/远程分支名] [本地分支名]	# -u = --set-upstream
git branch -u origin/dev1	
git branch --unset-upstream		# 撤销追踪
git push -u origin dev	# push时候建立跟踪
# 删除远程分支
git push [主机名] :[远程分支名]
git push origin :dev	# 把空分支推送到远程
git push origin --delete dev1 # 也可以删除
# 删除本地分支
git branch -D dev
```

### 多人协作

需要两个github账户

fork: 克隆一个到自己仓库中, 包括所有分支, commit, 但不包括issue. 原来版本变化, 自己仓库不变

1. 组长创建仓库, 添加合作者.

2. 合作者接受后, fork组长的仓库到自己仓库中.

3. 组长在仓库中加一些issue(项目任务或者待解决问题), 可以指派给合作者完成

4. 合作者在自己仓库完成后, git com 'fix #1 添加文件' , 这里fix #1 必须的, 组长仓库中的#1 issue会自动关闭

5. 合作者要pull request, 提交一个PR, 虽然自己也有merge权限, 但尽量不要自己来

6. 组长可以merge, 3种方法

   1. create a merge commit: 保留所有提交记录, 常用
   2. spuash and merge: 压缩成一次提交
   3. rebase and merge: 变成组长自己的提交

7. 组员同步组长仓库

8. ```shell
   git remote add up git@...   # 可以设置主机名是up/ upstream, 
   git fetch up 	# 让远程分支同步
   git pull --rebase up master
   git rebase up/master	# 同上, 修改自己的本地master分支, 变成up的那样
   ```

9. 也可以直接clone组长的, 不用fork这样麻烦



### git tag

当有阶段性版本, v1.0, v2.0这样, 对重大版本加上记号

注意tag是对于某次提交创建的, 和分支无关. 如果多个分支都有这个提交版本, 那么有这个提交相同的tag

```shell
git tag v1.0 -m "发布正式版本v1.0"
git show v1.0	# 查看详情
git tag -d v1.0  # 删除
git push origin v1.0	# 提交tag
git checkout -b v1.0	# 创建v1.0分支并进入
```

release: 发布软件产品的版本, 附带发布说明和源代码, 和tag绑定, 添加了编译好的文件



### gitmodule

在git管理项目中, 遇到公共库和基础工具, submodule来管理

主目录proj, 子目录lib

```shell
git submodule add [子项目网址] [目录]	
# 只是把submodule当前的commit id加入到父目录的索引
```

1. clone lib.git 到本地
2. 创建.gitsubmodule来记录相关信息
3. 更新.git/config 增加submodule地址

删除:

	1. 删除.git/config, .gitsubmodule部分
	2. git rm --cached [目录]

签出: 获得子目录内容

```shell
git submodule init
git submodule update
git submodule update --init --recursive # 组合命令
```

如果子目录有更新, 父目录中git pull 不会更新子目录, 可以git status查看下, git submodule update才更新

如果就在子目录中更新了, 父目录也要commit一下



### 安全

1. 更换电脑后，发现无法push，需要在本地电脑上生成ssh key，添加到github中

2. 以后git clone 尽量克隆 ssh链接的，https链接的需要使用什么令牌什么的，我的令牌是`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`

3. ```
   git remtoe set-url origin https://<token>@github.com/jensen-yan/<repo>.git
   
   ```

4. 
