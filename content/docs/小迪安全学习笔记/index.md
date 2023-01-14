---
title: "小迪安全学习笔记-基础入门"
date: 2023-01-08
tags: ["安全"]
series: ["小迪安全学习笔记"]
series_order: 1
---

## 概念名词

### 域名

**什么是域名**

**顶级域名、二级域名和多级域名**

**域名发现对安全测试的意义**

- 旁站攻击

### DNS

**什么是 DNS？**

- 域名系统（**D**omain **N**ame **S**ystem）。它是一个域名和IP地址相互映射的一个分布式数据库，能够使人更方便地访问互联网。DNS使用UDP端口53。对于每一级域名长度的限制是63个字符，域名总长度则不能超过253个字符。

**本地 HOSTS 与 DNS 的关系？**

- Hosts在本地将一些常用的网址域名与其对应的IP地址建立一个关联“数据库”，当我们访问域名时，系统会首先自动从Hosts文件中寻找对应的IP地址，一旦找到，系统会立即打开对应网页，如果没有找到，则系统会再将网址提交DNS域名解析服务器进行IP地址的解析。
- Hosts地址：C:\Windows\System32\drivers\etc\hosts

**CDN 是什么？与 DNS 的关系？**

- CDN：是构建在数据网络上的一种分布式的内容分发网。可以提高系统的响应速度，也可以一定程度的拦截/防御攻击。
- 在攻击的时候，如果找不到源站，那么我们扫描的就是CDN节点中的缓存，是一种阻碍。
- ![img](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301081015969.png)

**常见的 DNS 安全攻击有哪些？**

- 缓存投毒：它是利用虚假Internet地址替换掉域名系统表中的地址，进而制造破坏。
- DNS劫持：是指在劫持的网络范围内拦截域名解析的请求，分析请求的域名，把审查范围以外的请求放行，否则返回假的IP地址或者什么都不做使请求失去响应，其效果就是对特定的网络不能访问或访问的是假网址。（针对面较广）
- 域名劫持：域名劫持就是在劫持的网络范围内拦截域名解析的请求，分析请求的域名，把审查范围以外的请求放行，否则直接返回假的IP地址或者什么也不做使得请求失去响应，其效果就是对特定的网址不能访问或访问的是假网址。（针对面窄一点）
- DNS DDOS攻击：通过控制大批僵尸网络利用真实DNS协议栈发起大量域名查询请求，利用工具软件伪造源IP发送海量DNS查询，发送海量DNS查询报文导致网络带宽耗尽而无法传送正常DNS查询请求。

### 脚本语言

**常见的脚本语言类型有哪些？**

- asp php aspx jsp javaweb pl py cgi 等

**不同脚本类型与安全漏洞的关系？**

- 不同脚本可能爆发漏洞的可能性有所不同

- 不同脚本漏洞的存在点可能不同，因为不同语言的适用范围不同

### 后门

**什么是后门？**

- 通常指那些绕过安全性控制而获取对程序或系统访问权的程序方法。

- 在软件的开发阶段，程序员常常会在软件内创建后门程序以便可以修改程序设计中的缺陷。

**后门在安全测试中的实际意义？**

- 可以更方便的链接到主机

- 在获取到玩主机权限的时候，后门可以充当命令控制台的角色

### Web

**WEB 的组成架构模型？**

- 网站源码：分脚本类型，分应用方向
- 操作系统：windows linux
- 中间件（搭建平台）：apache iis tomcat nginx 等
- 数据库：access mysql mssql oracle sybase db2 postsql 等

**为什么要从 WEB 层面为主为首？**

- web使用的比较广
- web网站的漏洞相对较多
- web 作为跳板深入到其他资源相对容易

**WEB 相关安全漏洞**

> 找漏洞的时候从不同层面来入手

- WEB 源码类对应漏洞：SQL 注入，上传，XSS，代码执行，变量覆盖，逻辑漏洞，反序列化等
- WEB 中间件对应漏洞：未授权访问，变量覆盖...
- WEB 数据库对应漏洞：弱口令，权限提升...
- WEB 系统层对应漏洞：提权，远程代码执行
- 其他第三方对应漏洞
- APP 或 PC 应用结合类

### 其他

**APP网站套用**

- APP和网站用的是同一套源码，只是显示效果优化了

> 进程抓包工具：Wsexplorer

## 数据包拓展

> 代理、HTTP 与 HTTPS、Request 请求数据包数据格式、Response 返回数据包数据格式

### Request 数据包格式

> 请求行、请求头、空行、请求数据

**示例数据**

```http
POST /adduser HTTP/1.1
Host: localhost:8030
Connection: keep-alive
Content-Length: 16
Pragma: no-cache
Cache-Control: no-cache
Origin: chrome-extension://fdmmgilgnpjigdojojpjoooidkmcomcm
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/66.0.3359.181 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
```

**请求行**

- 请求行由三个标记组成：请求方法、请求 URL 和 HTTP 版本，它们用空格分割。
- 例如：`GET /index.html HTTP/1.1`

```
HTTP 规划定义了 8 种可能的请求方法：
GET：检索 URL 中标识资源的一个简单请求
HEAD：与 GET 方法相同，服务器只返回状态行和头标，并不返回请求文档
POST：服务器接受被写入客户端输出流中的数据的请求
PUT：服务器保存请求数据作为指定 URL 新内容的请求
DELETE：服务器删除 URL 中命令的资源的请求
OPTIONS：关于服务器支持的请求方法信息的请求
TRACE：web 服务器反馈 Http 请求和其头标的请求
CONNECT ：已文档化，但当前未实现的一个方法，预留做隧道处理
```

**请求头**

```
由关键字/值对组成，每行一对，关键字和值用冒号分享。请求头标通知服务器腾于客户端的功能和标识。
HOST: 主机或域名地址
Accept：指浏览器或其他客户可以接爱的 MIME 文件格式。Servlet 可以根据它判断并返回适当的文件格
式。
User-Agent：是客户浏览器名称
Host：对应网址 URL 中的 Web 名称和端口号。
Accept-Langeuage：指出浏览器可以接受的语言种类，如 en 或 en-us，指英语。
connection：用来告诉服务器是否可以维持固定的 HTTP 连接。http 是无连接的，HTTP/1.1 使用 Keep-Alive
为默认值，这样，当浏览器需要多个文件时(比如一个 HTML 文件和相关的图形文件)，不需要每次都建立
连接
Cookie：浏览器用这个属性向服务器发送 Cookie。Cookie 是在浏览器中寄存的小型数据体，它可以记载
和服务器相关的用户信息，也可以用来实现会话功能。
Referer ： 表 明 产 生 请 求 的 网 页 URL 。 如 比 从 网 页 /icconcept/index.jsp 中 点 击 一 个 链 接 到 网 页
/icwork/search ， 在 向 服 务 器 发 送 的 GET/icwork/search 中 的 请 求 中 ， Referer 是
http://hostname:8080/icconcept/index.jsp。这个属性可以用来跟踪 Web 请求是从什么网站来的。
Content-Type：用来表名 request 的内容类型。可以用 HttpServletRequest 的 getContentType()方法取得。
Accept-Charset：指出浏览器可以接受的字符编码。英文浏览器的默认值是 ISO-8859-1.
Accept-Encoding：指出浏览器可以接受的编码方式。编码方式不同于文件格式，它是为了压缩文件并加
速文件传递速度。浏览器在接收到 Web 响应之后先解码，然后再检查文件格式。
```

**空行**

- 最后一个请求头标之后是空行，发送回车符和退行，通知服务器以下不再有头标。

**请求数据**

- 使用 POST 传送，最常使用的是 Content-Type 和 Content-Length 头标。

### Response 数据包格式

一个响应由四个部分组成；状态行、响应头标、空行、响应数据。

1. 状态行：协议版本、数字形式的状态代码和状态描述，个元素之间以空格分隔
2. 响应头标：包含服务器类型、日期、长度、内容类型等
3. 空行：响应头与响应体之间用空行隔开
4. 响应数据：浏览器会将实体内容中的数据取出来，生成相应的页面

**HTTP 响应码：**

```
1xx：信息，请求收到，继续处理
2xx：成功，行为被成功地接受、理解和采纳
3xx：重定向，为了完成请求，必须进一步执行的动作
4xx：客户端错误
5xx：服务器错
```

## 搭建安全拓展

1. 基于域名和IP扫描目录的不同

   域名和IP地址都可以访问网站，但是有的时候两者指向的目录不同，通常IP指向的目录是域名的上一级，因此我们在扫描网站的敏感文件时，借助IP扫描会得到更多的信息。

   > 维护人员可能会将网站源码备份，然后放在上级目录，此时IP扫描就有可以扫到备份的源码。

2. web源码中有什么敏感文件？

   后台路径、数据库配置文件、备份文件等

3. 后缀名解析问题

   不同的后缀名可能会对应相同的解析方式

   ![image-20230112184533322](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121845401.png)

   也可以自己创建一个后缀名并为其配备相应的解析方式

   ![image-20230112184537564](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121845456.png)

4. 基于中间件的简要识别

   可以借助抓包找到网站使用的中间件平台，response数据包

   ![image-20230112185101647](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121851730.png)

5. 匿名访问

   ![image-20230112185213180](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121852241.png)

   ![image-20230112185255900](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121852955.png)

   如果不启用匿名访问，在访问时就会弹出如下的框

   ![image-20230112185417168](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121854225.png)

   > 匿名访问实际上是自动登陆了，以该用户名登陆的都会被赋予相同的权限（来宾权限）

6. IP验证、域名验证

   就是一个黑名单和白名单的设置

   ![image-20230112185702931](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121857978.png)

   ![image-20230112185800251](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121858293.png)

   ![image-20230112185829761](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121858829.png)

7. 后门权限问题

   有些目录（比如：放图片的目录images）没有执行权限，后门上传到该目录是没有用的（连接都连接不上），这时候我们的解决办法就是换目录。

   ![image-20230112190139703](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121901761.png)

   有可能连接上后门，但是不能写入文件，甚至是不能读取，这是因为访问身份权限不够。

   ![image-20230112185946971](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121859025.png)

## web源码拓展

1. 程序源码重要性

2. 程序源码获取途径

   1. 知名的可以直接搜

   2. 小众的分为合法和非法，两个渠道也有不同

      > 淘宝、闲鱼、菜鸟源码

3. 获取源码之后要进行源码审计、漏洞挖掘、漏洞测试

   分析其目录工作原理（数据库备份，bak 文件等）

   > 分析其目录工作原理（数据库备份，bak 文件等）
   >
   > 不同语言产生的漏洞类型参考：[5.1.2. 反序列化 — Web安全学习笔记 1.0 文档 (websec.readthedocs.io)](https://websec.readthedocs.io/zh/latest/language/php/unserialize.html) 

4. CMS识别技术

   CMS识别技术是指识别网站使用的内容管理系统（CMS）的技术。这种技术通常用于网络安全，网络渗透测试和网站挖掘等领域。

   CMS识别技术主要通过分析网站的源代码、HTTP头、页面元素、链接等信息来识别网站使用的CMS。有些CMS识别工具还可以通过扫描文件和目录结构来识别CMS。

   一些常见的CMS识别工具包括：

   - Wappalyzer：一款浏览器扩展，可以识别网站使用的技术，包括CMS等
   - WhatWeb：一款开源工具，用于识别网站使用的技术，包括CMS等
   - BuiltWith: 在线服务, 可以识别网站使用的技术, 包括CMS, 应用程序, 插件, 工具等

   注意，这些工具可能存在误报或漏报，需要进行人工识别或验证。

   人工识别：找到源码中疑似指纹的文件，用搜索引擎搜一下，可以初步判定是否可以用该文件作为整套源码的指纹。

   > CMS字典基本格式
   >
   > 文件路径|源码名字|MD5值
   >
   > 编写一个工具代码，对比MD5值可以判定源码

   现成工具和平台识别，面向大众，因此如果自己的目标网站比较小众，最好还是建一个属于自己的字典库。

   

![整体结构](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301141024660.png)

## 演示案例

### **子域名收集**

{{< article link="/posts/subdomian/" >}}

### **exe后门功能及危害**

[Releases · quasar/Quasar (github.com)](https://github.com/quasar/Quasar/releases)

生成`.exe`后门

![image-20230112102711405](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121027299.png)

开启端口监听

![image-20230112103557937](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121035103.png)

远程桌面，因为是127.0.0.1，所以会出现如下图所示的情况

![image-20230112103506320](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121035086.png)

其他功能

![image-20230112103724084](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121039771.png)

![image-20230112103640612](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301121036708.png)

{{< alert >}}

需要在沙盒或虚拟机中实验！

{{< /alert >}}





