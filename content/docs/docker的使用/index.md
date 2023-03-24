---
title: "docker命令"
date: 2023-01-07
tags: [‘docker’]
---

# docker命令

1. 查看镜像

   docker image ls

2. 删除所有镜像

   docker rmi $(docker images -q)

3. 查看运行中的容器

   docker ps

4. 宿主机和容器间复制文件

   从主机复制到容器`sudo docker cp host_path containerID:container_path`

   从容器复制到主机`sudo docker cp containerID:container_path host_path`

5. 停止并删除容器

   `docker stop <container id|name=``""``></container>`

   `docker rm <container id|name=``""``> <container id|name=``""``></container></container>`

6. 进入容器

   `docker exec -it 44fc0f0582d9 /bin/sh`

7. docker批量停止或删除容器

   stop停止所有容器 

   `docker stop $(docker ps -a -q)`

   remove删除所有容器

   `docker  rm $(docker ps -a -q)` 

8. 将docker容器设为自启动和取消容器自启动

   - 将正在运行的容器设为自启动

   `docker update --restart=always <CONTAINER ID>`

   - 将自启动的容器取消自启动

   `docker update --restart=no <CONTAINER ID>`

   `docker update --restart=no $(docker ps -a -q)`

# docker编辑文件

精简版没有vi也没有vim，那么要怎么编辑文件？

1. `echo abc >> test.txt` ``
2. 下载vim
3. 在宿主机编写好文件之后copy到容器中
4. 使用`sed`命令  [Linux sed 命令 | 菜鸟教程 (runoob.com)](https://www.runoob.com/linux/linux-comm-sed.html) 





# 重启docker服务

#### systemctl 方式

守护进程重启
 `sudo systemctl daemon-reload`
 重启docker服务
 `sudo systemctl restart docker`
 关闭docker
 `sudo systemctl stop docker`

#### service 方式

重启docker服务
 `sudo service docker restart`
 关闭docker
 `sudo service docker stop` 



