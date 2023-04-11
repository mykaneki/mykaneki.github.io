---
title: "upload-labs"
date: 2023-01-01
tags: [‘文件上传漏洞’,'漏洞']
draft: false
---

## 靶场搭建

https://github.com/c0ny1/upload-labs

## 基本思路

![](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303271550052.png)

![](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303271552220.png)

1. 禁用 JavaScript 

2. MIME 绕过

3. 扩展后缀名

4. 配置文件`.htaccess` 

   > 自建后缀名，通配后缀名
   >
   > 文件上传后文件名可能会修改，此时只要后缀名匹配了，一样可以识别
   >
   > webshell 工具可能没法利用部分已有的后缀，比如 `.jpg` 

5. 各种绕过

   1. 大小写绕过
   2. 在文件名的首尾去加点
   3. 在文件名的首尾加空格
   4. 在文件名的首尾加::$DATA

6. 

## Pass - 01

1. 客户端检查

2. 禁用 JavaScript 

   火狐禁用：打开设置，搜索JavaScript，关闭



## Pass - 02

1. 服务端检查

2. 可以上传非图片内容，检查后缀

3. 白名单：MIME 绕过

   抓包，将Content-Type修改为允许上传的类型（image/jpeg、image/png、image/gif）三选一

4. 菜刀连接

   木马内容 `<?php @eval($_POST['caidao']);?> `

5. 成功



## Pass - 03

1. 服务端检查

2. 上传php文件，黑名单

   提示：不允许上传.asp,.aspx,.php,.jsp后缀文件！

3. 扩展后缀名

   `php,phps,php3,php5,phtml`等都有可能被解析成`php`文件的

   {{< alert >}}
   是否能解析，取决与`apache`的配置
   {{< /alert >}}

4. 查看回显数据包，可知道文件名

5. 访问该文件

   很遗憾，并不能解析

   由于使用docker部署的，找配置文件找了很久，因为大多数资料都是Windows部署的

   最后使用了局部配置文件`.htaccess`

   文件内容为：

   ```php
   AddType application/x-httpd-php .php .phtml .phps .php5 .pht
   ```

   该文件放在`/var/www/html` 

   > 尝试放在 /var/www/html/Pass-03 并没有生效

6. 解析成功

   ![image-20230328000537929](C:/Users/27951/AppData/Roaming/Typora/typora-user-images/image-20230328000537929.png)

7. 这时候使用蚁剑和菜刀都可以连接了

   ![image-20230328000908996](C:/Users/27951/AppData/Roaming/Typora/typora-user-images/image-20230328000908996.png)

   

## Pass - 04

**方法一**

1. 服务端、后缀、黑名单
2. 尝试后缀`.phps`，上传成功
3. 查看文件

**方法二**

1. 上传`.htaccess`文件更改apache配置

   ```php
   <FilesMatch "01.xxx">
     SetHandler application/x-httpd-php
   </FilesMatch>
   ```

   这段代码的意思是把`01.xxx` 文件当作php文件执行

   `.htaccess`文件是隐藏文件，需要右键设置，显示隐藏文件

2. 成功

   ![image-20230328161921409](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303281619548.png)
   
   {{ alert }}
   
   ​	图片马不能直接利用，得结合文件包含漏洞
   
   ​	在使用 `01.jpg` 连接蚁剑是失败的，所以用了个 `.xxx` 的后缀
   
   {{ alert }}
   
   > 文件包含漏洞：本来想引用一个php文件，但是漏洞就是，他不会识别什么是php文件，只要是他引用的，他都当php来解析，所以如果他引用的是jpg，但是jpg中有图片马，那么他就相当于引用了图片马，同样的道理还会有zip马等等。



## Pass - 05

大小写绕过

![image-20230328164800536](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303281648582.png)



## Pass - 06

首尾加空格





