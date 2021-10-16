#### 常用命令

1. docker images 查看所有images
2. docker ps -a 查看所有containers， 包括运行，没运行的
3. docker run \<image name\> \<cmd> \<params>
4. docker run hello-docker --name hello  直接运行，并且把container命名为hello
5. docker run  -p 8080:80  -d  docker/getting-started  把本机8080端口映射到docker的80端口，这样本机localhost:8080能访问docker的浏览器内容。 -d detached 绑定到后台 
6. docker run --rm -it ubuntu /bin/bash  -i 交互式， -t 使用tty键盘交互， --rm 用完就扔了， /bin/bash 打开终端
7. docker stop hello 停止运行的container
8. docker start hello 重新运行
9. docker rm hello 删除container
10. docker rmi hello-docker  产出images
11. docker exec -it \<contariner id> bash 重新开一个bash终端



#### 远程

1. 我的doceker id 是jensenyan
2. docker pull ... 下载到本地
3. docker tag  hello jensenyan/hello  把hello创建一个标签
4. 需要在远程创建一个jensenyan/hello 仓库
5. docker push jensen/yan  把本地标签push上去



#### containers

1. 3个重要概念 

   1. namespace： 限制一个process能看到什么，例如文件系统，网络，pid等主机的一部分
   2. change the root
   3. control groups

   
