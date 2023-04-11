---
title: "重装电脑"
date: 2022-09-03
description: "因为电脑经常重装系统，所以写这篇文章，程序员的新电脑应该干些什么"
tags: ["git", "linux", "kali", "docker","wsl"]
showDate: true
---

## git安装与配置

1. [git下载](https://git-scm.com/download)

2. 配置账号信息

   ```git
   # git config --global user.name "m"
   # git config --global user.email "2795188612@qq.com"
   # ssh-keygen -t rsa -C "2795188612@qq.com"
   ```

   到git仓库，添加秘钥

   查看密钥存放路径

   ![image-20220930222249737](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209302222791.png)

   ​	将`.pub` 的内容复制到github上

3. GitHub上的git文档[git-tips/README.md at master · kwshare/git-tips (github.com)](https://github.com/kwshare/git-tips/blob/master/README.md#配置管理) 

4. 注意本地仓库和远程仓库位置的设置

## Win-KeX kali-WSL

[Win-KeX | Kali Linux Documentation](https://www.kali.org/docs/wsl/win-kex/)

1. 图形界面启动命令

​		`kex --win -s`

​		`kex --sl -s`

​		`kex --esm --ip -s 远程桌面连接 password:kali`

2. 修改root密码

   `sudo passwd root`

   `password:root`

### 一些小问题

1. 点击图标打不开wireshark，但是命令行可以

   > 使用命令：`sudo wireshark` 
   >
   > **如何使得普通用户能够启动wireshark?**
   >
   > 1. 将dumpcap的用户组更改为[wireshark](https://so.csdn.net/so/search?q=wireshark&spm=1001.2101.3001.7020)
   >
   >    `sudo chgrp wireshark /usr/bin/dumpcap`
   >
   > 2. 设置其他用户也具有与root一样的权限来执行dumpcap
   >
   >    `sudo chmod 4755 /usr/bin/dumpcap`
   >
   > 3. 将自身加入到wireshark组中,笔者的用户名为Jello,因此命令如下:
   >
   >    `sudo gpasswd -a Jello wireshark`

2. 配置WSL和宿主机互连，以及代理设置

   1. 修改防火墙规则（每次使用都要运行）

      `New-NetFirewallRule -DisplayName "WSL" -Direction Inbound  -InterfaceAlias "vEthernet (WSL)"  -Action Allow` 

      ![image-20221121110125897](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051813566.png)

      name :`{a64653d0-836e-4b5f-b6f0-dc1d058edca7}`

      然后会在Windows的防火墙高级设置的入站规则里会看到一条名为`WSL`的新规则

      ![image-20221121110220846](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051813463.png)

   2. vim ~/.local_profile
   
      ```sh
      export windows_host=`cat /etc/resolv.conf|grep nameserver|awk '{print $2}'`
      export ALL_PROXY=socks5://$windows_host:8888
      export HTTP_PROXY=$ALL_PROXY
      export http_proxy=$ALL_PROXY
      export HTTPS_PROXY=$ALL_PROXY
      export https_proxy=$ALL_PROXY
      
      if [ "`git config --global --get proxy.https`" != "socks5://$windows_host:8888" ]; then
                 git config --global proxy.https socks5://$windows_host:8888
      fi
      ```
   
3. 在powershell上固定kali终端

   点击设置 >> 左下角打开json文件 >> 进行编辑![image-20221130192757835](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051814095.png)

   ![image-20221130192904950](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051814761.png)

   ```json
   {
                   "commandline": "wsl -d kali-linux kex --esm --wtstart -s",
                   "guid": "{a64653d0-836e-4b5f-b6f0-dc1d058edca7}",
                   "hidden": false,
                   "icon": "file:///C:/Users/27951/kali-menu.png",
                   "name": "Win-KeX",
                   "startingDirectory": "//wsl$/kali-linux/home/kali"
               },
               {
                   "commandline": "wsl ~",
                   "guid": "{a64653d0-836e-4b5f-b6f0-dc1d058edca8}",
                   "hidden": false,
                   "name": "kali",
                   "startingDirectory": "%USERPROFILE%"
               }
   }
   ```
   
   

## Windows 访问 Linux 文件

**方法一**：通过 `\\wsl$` 访问 Linux 文件时将使用 WSL 分发版的默认用户。 因此，任何访问 Linux 文件的 Windows 应用都具有与默认用户相同的权限。

## Linux 访问 Windows 文件

在从 WSL 访问 Windows 文件时，可以直接使用`/mnt/{Windows盘符}`进入对应的盘中。

## kali虚拟机的安装和配置



1. 安装VMware

   > [阿里云盘分享](https://www.aliyundrive.com/s/TUkuP2hX3HD)
   >
   > VMware激活密钥（通用批量永久激活许可） 
   >
   > 16：ZF3R0-FHED2-M80TY-8QYGC-NPKYF 
   >
   > 15：FC7D0-D1YDL-M8DXZ-CYPZE-P2AY6 
   >
   > 12：ZC3TK-63GE6-481JY-WWW5T-Z7ATA 
   >
   > 10：1Z0G9-67285-FZG78-ZL3Q2-234JG

2. 下载kali镜像

   > 官网（解压即用）：https://kali.download/virtual-images/kali-2022.2/kali-linux-2022.2-vmware-amd64.7z
   >
   > 清华：https://mirrors.tuna.tsinghua.edu.cn/kali-images/kali-2022.2/kali-linux-2022.2-installer-amd64.iso

3. 更改分辨率（字太小了）

   > settings -> display


### 换源

> 1. 备份原先镜像源
>
> `sudo cp /etc/apt/sources.list  /etc/apt/sources.list.backup` 
>
> 2. 一键更换（更换成功的话后续操作就不需要了）
>
>    `sudo sed -i "s@http://http.kali.org@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list`
>
> 3. 输入`sudo vim /etc/apt/sources.list` 命令进入源地址文件
>
> 4. 按`i`进入插入模式
>
> 5. 选择以下任何一个源，复制
>
> ```cmd
> deb http://http.kali.org/kali kali-rolling main non-free contrib
> deb-src http://http.kali.org/kali kali-rolling main non-free contrib
> #tsinghua 清华
> 
> deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
> 
> deb-src http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
> 
> #aliyun 阿里云 
> 
> deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib 
> 
> deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib 
> 
> #ustc 中科大 
> 
> deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib 
> 
> deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib 
> 
> #浙大源
> 
> deb http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
> 
> deb-src http://mirrors.zju.edu.cn/kali kali-rolling main contrib non-free
> ```
>
> 
>
> 更新前把系统自带的更新源注释或者删除掉
>
> 5. Esc退出插入模式
>
> 6. 输入`:wq`保存退出
>
> 7. 最后分开输入`sudo apt-get update`和`sudo apt-get upgrade`两个命令，更新软件和升级
>
>    或者`apt-get update && apt-get upgrade && apt-get dist-upgrade` 
>



### 换语言

> 1. `dpkg-reconfigure locales` 更换
>
> 2. 找到`en_US.UTF-8 UTF-8`选项，按空格键将其进行取消
>
> 3. 找到`[ ]zh_CN.GBK_GBK` 和`[ ] zh-CN.UTF-8.UTF-8`两个选项，使用空格将`[ ]zh_CN.GBK_GBK` 和`[ ] zh-CN.UTF-8.UTF-8`其两项勾选上
>
> 4. `reboot` 重启
> 5. 将标准文件夹更新到当前语言吗？ 保留旧的名称

### 打开ssh

1. 测试是否打开了ssh

   ```bash
   ssh localhost
   ```

2. 配置ssh

   1. 打开配置文件`vim /etc/ssh/sshd_config` 

   2. 用于学习用途可以按以下配置

   ```bash
   PermitRootLogin yes              # 是否允许root用户登录，实际工作需要设置为no
   PubkeyAuthentication yes         # 是否开启基于公钥认证机制，有了公钥就可以免密登陆
   
   PasswordAuthentication yes        # 是否使用密码验证，如果使用密钥对验证可以关了它
   PermitEmptyPasswords yes          # 是否允许空密码，如果上面的那项是yes，这里最好设置no，但平时测试为了方便就设置了yes
   ```

3. 重启服务

   ```bash
   service ssh restart
   systemctl enable ssh
   ```

4. xshell连接测试

   ```bash
   ifconfig #查看IP地址
   ```

   ![Xshel连接](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051814640.png)

1. git配置（参考第一部分）

2. 安装docker

   1. `apt-get install docker docker-compose` 

   2. 启动docker服务

      ` service docker start`  

   3. 配置加速器

      ```bash
      vim /etc/docker/daemon.json
      #写入内容
      {
      	"registry-mirrors": ["https://xxx.mirror.aliyuncs.com"]
      }
      # https://cniirv3a.mirror.aliyuncs.com
      #重启
      systemctl restart docker
      #设置docker开机自启
      systemctl enable docker
      ```
      
      > 其他公开Docker镜像加速源（无需注册）
      >
      > https://docker.mirrors.ustc.edu.cn	#中科大
      >
      > http://hub-mirror.c.163.com/		#网易
      
      

   ## 虚拟机内软件

   1. hackbar2.1.3 （火狐插件）
   
      > 链接：https://pan.baidu.com/s/1evk5Vkxruh22bNEl_4qMVg?pwd=zyeu 
      > 提取码：zyeu

   2. [Burp_Suite_Pro_v1.7.37](https://www.iculture.cc/cybersecurity/pig=205) 

## 常用软件的安装与破解

### VM

安装VMware（见kali虚拟机的安装）

### typora安装与破解

「Typora1.3.8中文直装版.exe」https://www.aliyundrive.com/s/3ZTCfrrNJ27 

- 图床配置


### [Burp_Suite_Pro_v1.7.37](https://www.iculture.cc/cybersecurity/pig=205)

1. [下载jdk8](https://www.oracle.com/java/technologies/javase/javase8u211-later-archive-downloads.html)，选择Linux x86 Compressed Archive

   ![image-20221003120717176](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210031207261.png)

2. 下载并解压缩

   ```cmd
   #此处wget的非官方文件
   wget https://www.iculture.cc/software/tools/jdk-8u191-linux-x64.tar.gz
   tar -xzvf jdk-8u191-linux-x64.tar.gz
   sudo cp -r jdk1.8.0_191 /opt
   cd /opt/jdk1.8.0_191
   ```

3. 设置环境变量

   1. 编辑启动文件

> 有的是`bashrc` 有的是`zshrc`
>
> ``` cmd
> vim ~/.zshrc
> ```
>
> 在最下方添加
>
> > 目录要跟自己下载的jdk版本对应，不能盲目照搬
>
> ```cmd
> #install JAVA JDK
>  export JAVA_HOME=/opt/jdk1.8.0_191
>  export CLASSPATH=.:${JAVA_HOME}/lib
>  export PATH=${JAVA_HOME}/bin:$PATH
> ```
>
> 使其立即生效
>
> ```cmd
> source ~/.zshrc
> ```

   2. 安装

      ```cmd
      # sudo update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_191/bin/java 1
      # sudo update-alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_191/bin/javac 1
      # sudo update-alternatives --set java /opt/jdk1.8.0_191/bin/java
      # sudo update-alternatives --set javac /opt/jdk1.8.0_191/bin/javac
      ```

   3. 验证版本

      ```cmd
      java -version
      ```

4. 安装burpsuite专业版

   1. 下载

      > 1. 命令行`wget https://www.iculture.cc/software/tools/Burp_Suite_Pro_v1.7.37_Loader_Keygen.zip`
      >
      > 2. 若失效
      >
      >    >「Burp_Suite_Pro_v1.7.37_Loader_Keygen」等文件 https://www.aliyundrive.com/s/3cNMHv1G4h5 点击链接保存，或者复制本段内容，打开「阿里云盘」APP ，无需下载极速在线查看，视频原画倍速播放。

   2. 解压缩

      ```
      unzip Burp_Suite_Pro_v1.7.37_Loader_Keygen.zip
      ```

      将文件拷贝到`/usr/bin`目录中

      ```
      sudo cp -r burp-loader-keygen.jar burpsuite_pro_v1.7.37.jar /usr/bin
      ```

   3. 激活

       进入`/usr/bin`

       ```
       cd /usr/bin
       ```

       启动激活程序(jar程序启动通用命令)
       ```
       java -jar burp-loader-keygen.jar
       ```

       点击run

       点击next

       点击Manual activation完成手动激活

   4. 设置快捷方式

       1. 在/usr/bin中删除`burpsuite`社区版

          ```
          cd /usr/bin
          
          sudo rm -rf burpsuite
          ```

       2. 新建burpsuite

          ```
          vi burpsuite
          ```

       3. 设置如下内容

          ```
          #!/bin/sh
          
          java -Xbootclasspath/p:/usr/bin/burp-loader-keygen.jar -jar /usr/bin/burpsuite_pro_v1.7.37.jar
          ```

       4. 增加权限

          ```
          chmod +x burpsuite
          ```

       5. 进入`/usr/share/applications`

          ```
          cd /usr/share/applications
          ```

       6. 编辑`burpsuite`快捷方式

          ```
          vi kali-burpsuite.desktop
          ```

       7. 找到`Exec=sh -c`一行

          ```
          Exec=sh -c "java -jar /usr/bin/burpsuite"
          ```

       8. 修改为

          ```
          Exec=sh -c "/usr/bin/burpsuite"
          ```

          修改效果如图，请注意这里引号是英文的，别手抖打错了

          ![image-20220730150803354](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/image-20220730150803354.png)

## docker desktop

1. 配置镜像

   1. 获取自己的镜像地址

      > [容器镜像服务 (aliyun.com)](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors?accounttraceid=142fbc2277694d348b5082ac5e6cdab7lxyh) 

   2. 写入docker-setting

   ![镜像配置](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209301219149.png)

   3. 测试是否成功

      ```dockerfile
      docker info
      ```

      ![image-20220930122050978](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209301220049.png)

2. 以特权程序创建和运行容器，便于使用服务
   1. `docker run -d --name centos --privileged=true centos:centos7 /usr/sbin/init`
   2. `docker exec -it centos /bin/bash` 

## docker centos

1. `docker pull centos:latest `

2. Linux中必备常用支持库的安装

   在CentOS安装软件的时候，可能缺少一部分支持库，而报错。这里首先安装系统常用的支持库。那么在安装的时候就会减少很多的错误的出现。

   ```bash
   # yum install -y epel-release
   # yum install -y gcc gdb strace gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses ncurses-devel curl curl-devel e2fsprogs patch e2fsprogs-devel krb5-devel libidn libidn-devel openldap-devel nss_ldap openldap-clients openldap-servers libevent-devel libevent uuid-devel uuid mysql-devel    
   
   ```

3. 安装vim

   1. `yum -y install vim*` 

   ![开始](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209301224949.png)

   ![完成](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209301225149.png)

   2. 配置vim

      1. vim /etc/vimrc

      2. 找个位置添加以下代码

      ```bash
      set su
      set showmode
      set ruler
      set autoindent
      syntax on
      ```

      ![代码的意思](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209301232079.png)

      3. `:wq` 保存并退出

   3. 卸载

      `yum remove -y vim*`

4. 安装`ifconfig `、`firewall`、`nmcli` 、`nmtui`  

   1.  `yum install net-tools.x86_64`
   2. `yum install firewalld firewall-config` 
   3. `yum install NetworkManager` 
   4. `yum install NetworkManager-tui` 

5. 防火墙的关闭

   1. `systemctl stop firewalld`
   2. `systemctl disable firewalld.service` 





## WSL捣腾日记

> bug真的多，重试第三次，再不行就换Ubuntu

### 卸载子系统

![image-20221119110721646](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825459.png)

注销

`wsl --unregister kali-linux`

![image-20221119110827274](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825658.png)

### 下载kali-linux

![image-20221119111350541](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825452.png)

点击打开，设置`username`和`password`

>可以直接设置root密码
>
>`sudo -i`
>
>`passwd root`
>
>重复输入两次密码：`root`

根据kali官网文档[Win-KeX | Kali Linux Documentation](https://www.kali.org/docs/wsl/win-kex/#install-kali-linux-in-wsl2)

下载桌面版 

`sudo apt update`

`sudo apt install -y kali-win-kex` 

**第一次bug**

ping命令有问题

![image-20221119113920129](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825066.png)

**第二次bug**

在安装了完整版kali后，出现浏览器打不开的情况

**第三次重装**

ping依旧有问题

**第四次**

商店安装

设置初始`username`和`password`

更新源

测试，kali可以ping

安装桌面版

测试，kali不可以ping但是root可以，并且浏览器可以打开

![image-20221119131455621](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825912.png)

> **解决普通用户不能ping但是root可以的问题**
>
> **原因分析：** 
>
> ping命令在运行中采用了ICMP协议，需要发送ICMP报文。但是只有root用户才能建立ICMP报文。而正常情况下，ping命令的权限应为-rwsr-xr-x，即带有suid的文件，一旦该权限被修改，则普通用户无法正常使用该命令。
>
> **解决方案：** 
>
> 使用root用户执行
>
> `chmod u+s /bin/ping`
>
> ![image-20221119132942370](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825037.png)

**换源**

编辑 `/etc/apt/sources.list` 文件

```
deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main non-free-contrib
# deb-src https://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main non-free-contrib
```

`sudo apt-get update && apt-get upgrade && apt-get dist-upgrade` 

安装完整版kali

`sudo apt install -y kali-linux-large` 



### 安装LxRunOffline

> 可以迁移WSL的安装目录
>
> 参考教程
>
> [LxRunOffline使用手册 | 0opsdc (oopsdc.com)](https://oopsdc.com/post/lxrunoffline/#二安装lxrunoffline) 

[发布 ·DDoSolitary/LxRunOffline (github.com)](https://github.com/DDoSolitary/LxRunOffline/releases)

下载[LxRunOffline-v3.5.0-msvc.zip](https://github.com/DDoSolitary/LxRunOffline/releases/download/v3.5.0/LxRunOffline-v3.5.0-msvc.zip) 

解压文件到`C:\Windows\System32` 

命令行输入`lxrunoffline`，检测是否安装成功

**遇到了一个bug，删除了注册表，然后貌似一个问题解决了，又出现了新的问题**

> 问题链接
>
> [[ERROR\] Couldn't get the value "DistributionName" of the registry key "Software\Microsoft\Windows\CurrentVersion\Lxss\TryStoreWSL". · Issue #195 · DDoSolitary/LxRunOffline (github.com)](https://github.com/DDoSolitary/LxRunOffline/issues/195)

**使用离线包安装ubuntu**

如果是从微软官方下载`WSL`离线包，文件后缀为`.appx`，我们手动改为`.zip`，然后解压，`install.tar.gz`就是我们后续使用的安装文件。

![image-20221119120517839](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825229.png)

>lxrunoffline i -s -n <WSL名称> -d <安装路径> -f <安装包路径>.tar.gz
>
>lxrunoffline i -s -n ubuntu-linux -d E://WSL -f E:\Users\27951\Downloads\Compressed\CanonicalGroupLimited.UbuntuonWindows_2004.2021.825.0\Ubuntu_2004.2021.825.0_ARM64\install.tar.gz
>
>> -s 参数表示在桌面创建WSL快捷图标
>
>不出意外，就出意外了
>
>报错
>
>![image-20221119120838971](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825580.png)
>
>我猜这个是因为注册表的问题

解决不了，弃用

### 安装Windows Subsystem for Linux

![image-20221119110039983](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825910.png)

更新wsl

wsl.exe --update

### 商店Ubuntu20.04

设置账号密码

换源

```bash
sudo sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
```

更新源

`apt update`

安装图形界面

`sudo apt install -y xfce4 xrdp`

修改xrdp默认端口

```awk
sudo vim /etc/xrdp/xrdp.ini
# port=3390
```

为当前用户指定登录session类型

> 注意这一步很重要,如果不设置的话会导致后面远程桌面连接上闪退

```bash
sudo vim ~/.xsession

# 写入下面内容(就一行)
xfce4-session
```

###  启动xrdp

由于WSL2里面不能用`systemd`,所以需要手动启动

```awk
sudo /etc/init.d/xrdp start
#正常的话，返回如下：
 * Starting Remote Desktop Protocol server 
```

**远程访问**

​    在Windows系统中运行`mstsc`命令打开远程桌面连接,地址输入`localhost:3390`

注意这里的端口号应当与上面修改配置中一致

![image-20221119124225714](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301051825829.png)

**下载网络工具**

`sudo apt install net-tools`

> 这个ping命令没有问题
>
> 但是也打不开浏览器，奇奇怪怪

卸载了

## 局域网科学网络共享

1. 把电脑的ip改为固定的ip

2. clash打开局域网模式

3. 给手机配置代理

   
