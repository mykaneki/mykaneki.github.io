---
title: "子域名收集姿势总结"
date: 2023-01-10
tags: ["子域名","安全"]
series: ["网络安全"]
series_order: 1
---

## **为什么需要进行子域名收集？**

- 扩大资产范围，增加漏洞发现的概率

- 众所周知，一般情况下主站的安全性可能相对较高，而一些不常用的子站或者上线不久的站点，可能安全方面的考虑还没有很周全，可能成为目标系统的脆弱点

- 通常情况下，同一组织采用相同应用搭建多个服务的可能性很大，以及补丁的情况也可能大致相同，因此，存在相同漏洞的概率非常大

## 子域名收集的方式

子域名收集通常分为两种方式，分别为被动收集和主动收集。

被动收集是指，在不与目标系统进行交互的情况下，通过第三方进行收集。这种方式有着明显的优势，因为不需要和目标系统进行交互，所以不会对目标系统造成任何影响，更不会触发任何安全产品的告警。

> **被动子域名收集的方式：**
>
> - 信息泄露
> - 搜索引擎
> - 网络空间测绘引擎（网络资产搜索引擎）
> - 证书透明
> - 第三方DNS服务
> - AS 号码查询
> - SAN 收集
> - 使用公共数据集

主动收集是指，通过与目标系统进行交互，对子域名进行收集。因为需要和目标系统进行交互，很可能出现高频访问等情况，有触犯安全产品告警的风险。

> **主动收集子域名的方式：**
>
> - 字典枚举
> - 置换扫描
> - 域传送漏洞
> - DNSSEC
> - 域名缓存侦测技术

## 被动子域名收集

### 信息泄露

- corssdomain.xml
  跨站策略文件，主要是为web客户端(如Adobe Flash Player等)设置跨域处理数据的权限，里面可能包含部分域名信息。
- Github 、Gitee等代码仓库中，可能有相关子域名的信息
- 抓包分析获取，如一些静态资源的请求、一些APP或者小程序接口、邮件服务器等等

### 搜索引擎

常用的搜索引擎有Google和百度，基础的搜索语法：

```
site:*.baidu.com
```

{{<alert>}}

缺点：一些访问量较小的网址搜不到

{{</alert>}}

### 网络空间测绘引擎

>常见的空间测绘引擎：
>
>- [Shodan](https://exploits.shodan.io/welcome)
>- [Zoomeye](https://www.zoomeye.org/)
>- [Fofa](https://fofa.info/)

{{< article link="/posts/networkmapping/" >}}

### 证书透明

证书透明(Certificate Transparency) 是一种机制，用于验证证书是否被可信的证书颁发机构所颁发，并将所有颁发的证书公开记录在公共日志中，这些日志是由独立第三方维护的。

子域名收集可以利用证书透明日志中的信息来获取.

具体做法:

- 查询证书透明日志，获取指定域名对应的证书信息。
- 从证书信息中提取出网站的域名（包括子域名）。
- 将所有提取出的域名进行去重和排序，得到该域名下的所有子域名。

证书透明提供了一种更加安全可靠的子域名收集方式,因为它是基于证书的机制，可以在发现子域名的同时验证子域名的合法性，从而减少收集错误的子域名或者欺骗性子域名的可能性.

>常用证书透明查询网站
>
>- censys： https://censys.io/certificates
>- crtsh： https://crt.sh/
>- spyse： https://spyse.com/search/certificate
>- certspotter： https://sslmate.com/certspotter/api/
>- entrust： https://www.entrust.com/ct-search/
>- facebook： https://developers.facebook.com/tools/ct

### 第三方DNS服务

> [VirusTotal](https://www.virustotal.com/gui/home/search)

VirusTotal 在执行 URL 扫描时会使用一个功能叫做 "DNS 复制" 来收集和分析网络数据。

DNS (域名系统)是一种分布式的解析系统,它将域名映射到对应的IP地址。当一个用户访问一个 URL，他的设备需要通过DNS来解析对应的 IP 地址。而DNS复制功能就是利用这个过程来收集所有用户访问 URL 时执行的DNS解析信息。

这些信息包括 URL，对应的IP地址,DNS服务器等, 最后会存储到 VirusTotal 的数据库中供用户使用. 这些信息可以帮助用户更好的分析和了解URL访问的相关情况，如可疑的URL,域名查询等。

![image-20230111112039234](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301111120320.png)

> 其他在线DNS工具汇总：
>
> - https://decoder.link/
> - https://searchdns.netcraft.com/
> - https://dnsdumpster.com/
> - https://pentest-tools.com/information-gathering/find-subdomains-of-domain
> - https://www.pkey.in/tools-i/search-subdomains
> - https://hackertarget.com/find-dns-host-records/
> - https://findsubdomains.com/
> - https://spyse.com/

### AS 号码查询

ASN(Autonomous System Number) 是由网络协议路由部门(Internet Assigned Numbers Authority, IANA)分配的标识自治系统的数字。

自治系统（AS）是由一组网络路由器组成的网络系统。 它们通常由一个运营商或组织管理，并且是由一个或多个路由器组成的。

ASN是唯一的、全局的标识符，用于标识自治系统的身份，主要用于路由和BGP协议中，它用于在路由器之间确定网络间的路由关系。

使用 ASN 可以帮助收集子域名的方法主要有两种：

1. 利用 BGP 协议收集子域名: 在 BGP 协议中，路由器之间交换的信息包含了该 ASN 所管辖的 IP 地址段。可以通过分析 BGP 协议信息来发现与该 ASN 相关的子域名。
2. 利用已有的 ASN 数据库收集子域名:有一些第三方的数据库如 Team Cymru，他们提供了将 IP 地址映射到 ASN 信息的服务，可以通过这些数据库来查询与特定 ASN 相关的子域名。



### SAN 收集

SAN (Subject Alternative Name) 是一种在 X.509 证书中使用的扩展域，用于定义证书拥有者的多个域名。它可以用于在一个证书中为多个域名（包括子域名）提供安全认证。

SAN证书在构建多域名的网站的时候有重要的作用,这样就不用再申请多个证书,只需要一个证书就可以保证多个域名和子域名的安全连接,这样可以节省成本,提高效率.

同样的，可以利用SAN证书来收集子域名。可以分析证书上所包含的域名，来获得子域名信息。一般来说，需要使用特殊的工具或者脚本来分析证书中的信息。

可以使用以下步骤来利用 SAN 证书来收集子域名:

1. 使用证书颁发机构的查询工具或者第三方的工具来检索指定域名的证书。
2. 从获取的证书信息中提取出证书的 Subject Alternative Name (SAN) 域。
3. 从 SAN 域中提取出包含子域名的信息，去重、排序。
4. 得到的子域名信息可以用于进一步的安全分析和管理。

![image-20230111115959224](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202301111159291.png)

### 使用公共数据集

利用已有公开的扫描数据集，对子域名信息进行收集。

通常这些数据集都是比较大，可以使用命令进行快速查找

```bash
wget https://scans.io/data/rapid7/sonar.fdns_v2/20170417-fdns.json.gz
cat 20170417-fdns.json.gz | pigz -dc | grep ".Your_Target.org" | jq
```

## 主动子域名收集

### 字典枚举（暴力破解）

优点是可以快速发现子域名，无需依赖其他工具和服务；

缺点是能收集到多少子域，取决于字典的覆盖程度，并且需要大量的带宽和计算资源，还有可能被目标网站的防护机制阻止。

### 置换扫描

使用已知域/子域名的排列组合来识别新的子域名，通过对目标域名进行字符置换来生成可能的子域名进行枚举。使得字典有一定的针对性，提高准确率

比较常用的工具是 [altdns](https://github.com/infosec-au/altdns)

### 域传送漏洞

域传送漏洞 (Domain Transfers Vulnerabilities) 是一种安全漏洞,允许攻击者获取一个域名注册商的用户帐户的详细信息, 包括联系人信息，账单信息以及域名管理权限.

DNS区域传输是将DNS数据库或DNS记录从主名称服务器复制到辅助名称服务器的过程。如果DNS服务器没有进行严格的配置，只要收到AXFR请求就进行域传送，便造成了该漏洞。

域传送漏洞的影响可能包括:

- 攻击者可以获取域名注册商账户中的敏感信息，如联系人信息、账单信息等。
- 攻击者可以获取对域名的管理权限，如修改DNS记录，甚至指向恶意网站等.
- 攻击者可以篡改域名所有者信息，控制域名以及域名网站的所有权。

{{< alert >}}

请注意，使用域传送漏洞来收集子域名是非法行为，并且有可能会引起法律问题。此外，攻击域名注册商帐户可能会破坏网络的安全性并对其他客户造成影响。

{{< /alert >}}

域传送漏洞的即常见验证方式：

#### nslookup

```
# 1.nslookup命令进入交互式shell
$ nslookup

# 2.server命令 参数设定查询将要使用的DNS服务器
$ server xxx.com

# 3.如果漏洞存在的话，可以使用ls命令列出所有域名
$ ls

# 4.退出
$ exit
```

#### dig

在Linux下，可以使用dig命令来发送DNS请求，这里只需要发送axfr类型的DNS请求，如果存在该漏洞，则会返回所有解析记录。

```
dig @Target_DNS_Server_IP axfr 查询的域名
```

#### dnswalk

在Kali中已经预装的工具，这里需要注意的是，要使用域名的完整形式，即 `.`不能省略

```
dnswalk your_domain.
```

#### nmap

namap 中也有域传送漏洞的检测脚本：

```
nmap --script dns-zone-transfer --script-args dns-zone-transfer.domain=Your_domain -p 53 -Pn dns.xxx.yyy.com
```

### DNSSEC

DNSSEC (Domain Name System Security Extensions) 是一种用于增强 DNS 的安全性的扩展机制,通过在 DNS 中使用数字签名来验证 DNS 数据的完整性.

DNSSEC 主要用于防止 DNS 劫持和缓存污染攻击,保护用户访问网站的安全性.

DNSSEC 可以用来收集子域名,主要有两种方法:

1. 使用DNSSEC验证工具: 通过执行DNSSEC验证来验证域名的DNSSEC配置,从而获取域名的子域名.
2. 使用DNS枚举工具: 通过DNS枚举工具来枚举DNSSEC签名的域名中的所有子域名。

使用DNSSEC来收集子域名，能保证所收集的子域名是经过验证的，有保证的准确性。

常用的工具：

- ldns-walk
  可以使用 apt-get 安装

```bash
apt-get install ldnsutils
```

- [nsec3walker](https://dnscurve.org/nsec3walker.html)
- [nsec3map](https://github.com/anonion0/nsec3map)

### 域名缓存侦测

域名缓存侦测(DNS Cache Snooping) 是一种技术，通过利用DNS缓存来发现被缓存的域名和子域名的技术。

这种技术通过发送伪造的DNS请求来探测目标域名的缓存,并在缓存中寻找有用的信息。

一般来说, 域名缓存侦测技术需要两个部分：

- 执行DNS请求: 发送一些伪造的DNS请求来探测缓存中的域名和子域名.
- 解析缓存信息: 解析缓存中所有的DNS记录来获取子域名信息.

域名缓存侦测技术具有较高的收集子域名效率，在短时间内可以发现大量子域名，但它也有一些缺点:

- 只能发现已经被缓存的域名和子域名,无法发现新的域名和子域名
- 在缓存过期或清除后将无法获取到有用的信息
- 可能会被检测到并被阻止
- 可能会对目标网络造成较大的流量和负担.

{{< alert >}}

需要注意的是域名缓存侦测技术可能会对目标网络造成压力，需要在使用时谨慎考虑。

{{< /alert >}}

## 自动化工具

1. [Aquatone](https://github.com/michenriksen/aquatone)：用于自动收集子域名, 生成漂亮的网站快照

2. [OneForAll](https://github.com/shmilylty/OneForAll)：是一款比较综合的子域名扫描工具，拥有多个模块和接口扫描，收集子域信息很全，包括子域、子域IP、子域常用端口、子域Title、子域Banner、子域状态等。

    > 支持docker
    >
    > 中文友好
