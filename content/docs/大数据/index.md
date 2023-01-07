---
title: "大数据基本配置和使用"
date: 2022-01-07
draft: false
tags: ["大数据"]
---

## 集群配置

kali+docker

1. 选择一个目录存放git clone的目录，克隆项目

   > 项目克隆前需要换源以及git ssh配置

`cd ~/clonprj`

`git clone git@github.com:big-data-europe/docker-hadoop.git` 

`cat docker-compose.yml` 查看资源清单

```yaml
version: "3"

services:
  namenode:
    image: bde2020/hadoop-namenode:2.0.0-hadoop3.2.1-java8
    container_name: namenode
    restart: always
    ports:
      - 9870:9870
      - 9000:9000
    volumes:
      - hadoop_namenode:/hadoop/dfs/name
    environment:
      - CLUSTER_NAME=test
    env_file:
      - ./hadoop.env

  datanode:
    image: bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8
    container_name: datanode
    restart: always
    volumes:
      - hadoop_datanode:/hadoop/dfs/data
    environment:
      SERVICE_PRECONDITION: "namenode:9870"
    env_file:
      - ./hadoop.env
  
  resourcemanager:
    image: bde2020/hadoop-resourcemanager:2.0.0-hadoop3.2.1-java8
    container_name: resourcemanager
    restart: always
    environment:
      SERVICE_PRECONDITION: "namenode:9000 namenode:9870 datanode:9864"
    env_file:
      - ./hadoop.env

  nodemanager1:
    image: bde2020/hadoop-nodemanager:2.0.0-hadoop3.2.1-java8
    container_name: nodemanager
    restart: always
    environment:
      SERVICE_PRECONDITION: "namenode:9000 namenode:9870 datanode:9864 resourcemanager:8088"
    env_file:
      - ./hadoop.env
  
  historyserver:
    image: bde2020/hadoop-historyserver:2.0.0-hadoop3.2.1-java8
    container_name: historyserver
    restart: always
    environment:
      SERVICE_PRECONDITION: "namenode:9000 namenode:9870 datanode:9864 resourcemanager:8088"
    volumes:
      - hadoop_historyserver:/hadoop/yarn/timeline
    env_file:
      - ./hadoop.env
  
volumes:
  hadoop_namenode:
  hadoop_datanode:
  hadoop_historyserver:

```

`docker-compose build` 项目构建

`docker-compose up -d` 自动安装依赖

`docker network list` 查看集群的PID

` docker network inspect ID` 查看各个容器是什么IP

使用网页访问hadoop

> 每次重启电脑都有可能变化，即使虚拟机挂起，也会遇到问题

- Namenode: http://<dockerhadoop_IP_address>:9870/dfshealth.html#tab-overview
- History server: http://<dockerhadoop_IP_address>:8188/applicationhistory
- Datanode: http://<dockerhadoop_IP_address>:9864/
- Nodemanager: http://<dockerhadoop_IP_address>:8042/node
- Resource manager: http://<dockerhadoop_IP_address>:8088/

由于kali虚拟机和宿主机不通，所以需要在kali里面访问web页面

![http://172.18.0.2:9870/](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210021945671.png)

选择一个结点，进入执行命令

`docker exec -it c8s /bin/bash` 

查看hdfs的位置

`which hdfs` 

查看命令

`/opt/hadoop-3.2.0/bin//hdfs dfs` 

执行命令

`/opt/hadoop-3.2.0/bin//hdfs dfs -mkdir -p /datas/test/input` 

or

`hadoop fs -mkdir /sanguo` 

##  hdfs命令学习

```shell
# 需要先把宿主机的文件移到容器中，然后将容器的文件上传到hadoop中
# 也可以直接在容器中新建文件，只是docker的容器都是精简版，没有vi也没有vim
# 在指定的文件夹存放需要复制的文件
# 新建目录input和文件sanguo.txt
cd /opt/module/hadoop-3.2.0/
mkdir input
cd input
vim sanguo.txt
# 查看目标容器ID
docker ps -a
# 从宿主机复制文件到容器
docker cp /opt/module/hadoop-3.2.0/input/ 51ca5b603c74:/input/sanguo/
# 然后将容器的文件上传到hadoop中
# 以下命令在容器操作
# 选择容器的文件上传到集群
# 剪切
hadoop fs  -moveFromLocal  ./input/shuguo.txt  /sanguo
# 复制上传
hadoop fs -put ./input/sanguo / #这会把sanguo包括其内部的文件全部递归上传 /sanguo/shuguo.txt
hadoop fs -put /input/sanguo /sanguo #这会导致两层目录重名 即/sanguo/sanguo/shuguo.txt
# 删除目录和文件
hadoop fs -rm -r /sanguo
hadoop fs -rm /sanguo/shuguo.txt
# 追加文件到已经存在的文件末尾
hadoop fs -appendToFile /input/sanguo/liubei.txt /sanguo/shuguo.txt
# 提升文件权限
hadoop fs -chmod 666 /sanguo/shuguo.txt
# 修改文件所有者owner、所有群group
hadoop fs -chown kali:kali /sanguo/shu
guo.txt
# 修改副本数量
hadoop fs -setrep 10 /jingguo/shuguo.txt
# Replication 10 set: /jingguo/shuguo.txt
#这里设置的副本数只是记录在NameNode的元数据中，是否真的会有这么多副本，还得看DataNode的数量。因为目前只有3台设备，最多也就3个副本，只有节点数的增加到10台时，副本数才能达到10。


```

![修改文件所有者、所有群](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210021649846.png)

![image-20221002171121099](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210021711173.png)

![大文件上传示例](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210021918873.png)

> 比较大的文件上传会分为多个block

## 执行命令时遇到的问题

- put时遇到的error

  ```error
  put: File /sanguo/shuguo.txt._COPYING_ could only be written to 0 of the 1 minReplication nodes. There are 0 datanode(s) running and 0 node(s) are excluded in this operation.
  ```

  前一天晚上为了方便，直接挂起虚拟机，以为重新打开各个结点分配的IP不会变，直接打开web就可以，不需要一个个网址重新输入，事实证明还是想的太简单了，不仅网址无法连接，连使用命令上传文件也发生了问题，网上搜是需要format啥的，比较麻烦，风险又大，我可不想好不容易搭建起来的环境崩溃。

  于是我想着既然昨天没问题而虚拟机也没有关机，问题一定出在挂起。虽然挂起可以保存虚拟机离开时的状态，但是宿主机为其分配的存储空间不同，宿主机关机了之后会重新给虚拟机分配存储空间，进而影响到docker容器，因此需要重启分配存储空间。

  原因：**HDFS重新做了格式化，导致版本不一致** 

  解决：重启虚拟机

- put时遇到的一个INFO，经过搜索，这是正常的，不用理会

![image-20221002092838282](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210020928389.png)

- 追加文件时出现错误

```error
appendToFile: Failed to replace a bad datanode on the existing pipeline due to no more good datanodes being available to try.
```

解决办法：hadoop 解压路径下 / etc/ hadoop / hdfs-stie.xml

添加以下配置内容

```xml
<!-- appendToFile追加 -->
<property>
        <name>dfs.support.append</name>
        <value>true</value>
</property>

<property>
        <name>dfs.client.block.write.replace-datanode-on-failure.policy</name>
        <value>NEVER</value>
</property>
<property>
        <name>dfs.client.block.write.replace-datanode-on-failure.enable</name>
        <value>true</value>
</property>
```



## kali内直接安装hadoop

1. 官网下载hadoop-3.2.0.tar.gz

2. 将文件解压到/opt/module/hadoop-3.2.0/

   ` tar -zxvf hadoop-3.2.0.tar.gz -C /opt/module/`

   这里试过解压到`/home/kali/Downloads`下，最后的hadoop只能在特定目录下使用不能全局使用，因此就改回了`/opt/module` 

3. 配置环境变量

   `vim ~/.local_profile`

   `vim /etc/profile`

   `vim ~/.zshrc`

   `vim ~/.bashrc`

   `cd /etc/profile.d`

   

   > 为什么可以在这里面配置环境？而不是/etc/profile
   >
   > 查看shell脚本可以发现，它循环遍历profile.d里所有.`sh`后缀的文件，将其作为环境变量。
   >
   > ![image-20221001102137859](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210011021896.png)

   `vim my_env.sh` 没有文件会新建

   修改文件内容

   ```bash
   #HADOOP_HOME
   export HADOOP_HOME=/opt/module/hadoop-3.2.0
   export PATH=$PATH:$HADOOP_HOME/bin
   export PATH=$PATH:$HADOOP_HOME/sbin
   
   #JAVA_HOME
   #java的位置可以使用命令where java 查看，根据实际情况修改
   export JAVA_HOME=/opt/jdk1.8.0_191
   export PATH=$PATH:$JAVA_HOME/bin
   ```

4. 使配置文件生效

   `source /etc/profile`

   `source ~/.zshrc`

5. 测试，使用命令`hadoop`

​		若没有多出来一个#符号则成功![image-20221001100001079](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210011000120.png)

![hadoop文件](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210010922464.png)

bin 存放命令

![image-20221001092450680](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210010924742.png)

hdfs 存储

mapred 计算

yarn 资源调度



/etc/hadoop

![配置文件](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210010928024.png)

 include 类似于头文件

![image-20221001092940863](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210010929905.png)



./lib/native本地动态连接库

![image-20221001093121824](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210010931874.png)

sbin

![image-20221001093544490](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210010935587.png)

share 学习资料，包括文档和官方案例

![image-20221001093709937](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210010937986.png)

官方样例

![image-20221001093756440](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210010937515.png)




