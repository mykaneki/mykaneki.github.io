---
title: "Kali Linux 环境配置"
date: 2023-01-07
tags: ["kali"]
---

# kali linux 环境配置

起源于一次hadoop的安装和环境配置

因为环境调了很久，具体的细节记不清了，

一直在几个文件中反复横跳

`vim ~/.local_profile`

`vim /etc/profile`

`vim ~/.zshrc`

`vim ~/.bashrc`

`vim /etc/profile.d/my_env.sh`

期间出过一些异常情况：

1. 重启控制台环境就失效

2. xshell下使用普通用户kali运行hadoop可以识别命令，但是同一时间在虚拟机中无法识别命令；

3. 普通用户可以识别但是root不能识别，或者root能识别但是普通用户不能识别

4. 重启之后普通用户的JAVA_HOME不见了，但是root的还在

5. source了之后控制台发生变化

   ![前面的用户和路径都消失了](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210011346000.png)

6. 猜测`source /etc/profile`和`source ~/.zshrc` 可以改变当前的环境，因此会出现`source ~/.zshrc`了之后连sudo和vim都找不到



以上的原因大概就是使用的shell程序不同，对应的环境也不同，那么就应该统一环境

参考一篇博客，将zsh和bash的环境链接到一个我们自己新建的文件，以后可以在新建的文件里更改环境，两个都能生效。

1. 创建公共脚本

   ```bash
   touch ~/.local_profile
   ```

2. 在其中写入环境变量

   `vim ~/.local_profile` 

   ```bash
   #HADOOP_HOME
   export HADOOP_HOME=/opt/module/hadoop-3.2.0
   export PATH=$PATH:$HADOOP_HOME/bin
   export PATH=$PATH:$HADOOP_HOME/sbin
   
   # go 仅用于测试
   go_bin="my_env:/home/lemon/software/go/bin"
   export PATH=$PATH:$go_bin
   # test
   test="my_env:/home/lemon/software/test/bin"
   export PATH=$PATH:$test
   ```

3. 添加bash的引用

   `vim ~/.bashrc`

   在最后添加

   ```shell
   [[ -f ~/.local_profile ]] && . ~/.local_profile
   ```

4. 添加zsh的引用

   `vim ~/.zshrc`

   在最后添加

   ```shell
   # Use local customer env
   if [[ -f ~/.local_profile ]]; then
   source  ~/.local_profile
   fi
   ```

5. 生效终端配置

   ```bash
   source ~/.zshrc
   source /etc/profile
   su kali
   env | grep test
   ```

   ![image-20221001141052797](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202210011410843.png)



> 在虚拟机中`env | grep test` 结果一样，配置成功
>
> 之所以在虚拟机中测试一遍是因为我怀疑xshell使用的是bash的环境，而kali中是zsh，但是使用`echo $SHELL` 命令，结果均为zsh，除了环境，我不知道有什么能够造成两边结果不同。
