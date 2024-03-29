---
title: "计算机网络复习"
date: 2022-01-07
draft: false
tags: ["计网"]
---

## 思维导图

![image-20230324144404831](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202303241444940.png)

在notion查看

https://opposite-approval-0f0.notion.site/bb2e7dc1c680493e954b6804910d4c0f



## 计网英文复习

## chapter 1 Computer Networks and the Internet

> [chapter 1 Computer Networks and the Internet](https://blog.csdn.net/qq_48930699/article/details/121053676?spm=1001.2014.3001.5506)

- interconnects 互相连接

- Intranet 内网

- twisted-pair copper wire 双绞铜线

- a coaxial cable 同轴电缆

- fiber optics 光纤

- digital satellite channel 数字卫星信道

- guided media 导引性介质

- geostationary satellite 地球静止卫星

- low-altitude satellite 低空卫星

- low-earth orbiting 近地轨道

- message 报文 （应用层）

- frames 帧 （链路层）

- Segments 报文段 （传输层）

- Datagrams 数据报 / 分组（网络层）

- bit streams 比特流 （物理层）

- circuit-switched networks 电路交换网络

- packet Switching 分组交换

- overwhelms 淹没

- Congestion control 拥塞控制

- Store-and-forward transmission 存储转发传输

  > receive the **entire packet** before it can begin to transmit the first bit of the packet onto the outbound link（出站链路）.

- processing delay 处理时延

- propagation delay 传播时延（和距离有关）

- examine 核验

- traffic intensity 流量强度

- fraction 比率

- loosely classified into three categories 大致划分为三种类型

- no greater than 不大于

- equivalent concept 相同的概念

- residential access 住宅接入

- distributed applications 分布式应用程序

- corporate 公司

- connection-oriented 面向连接的

- analog signals 模拟信号

- proper order 按序

- gridlock 交通堵塞

- approaches 构建

- along a path 保留一条链路 （电路交换的特点）

- demand 需要

- as a consequence 因此

## Chapter 2 Application Layer

>[Chapter 2 Application Layer](https://blog.csdn.net/qq_48930699/article/details/121064380?spm=1001.2014.3001.5506)

- is labeled as 被标记为

  > client process: process that initiates communication. 
  >
  > server process:process that waits to be contracted.

- loss-tolerant application 遗失容忍性App

  > real-time audio/video(实时音视频) 
  >
  > stored audio/video（存储音视频）
  >
  > interactive games.（互动游戏）

- bandwidth-sensitive 带宽敏感

- elastic application 弹性应用（对吞吐量要求不高）

- underlying transport protocol 基础传输协议

- persistent connection without pipelining 流水线持久连接

  > [RTT的计算](https://blog.csdn.net/qq_48930699/article/details/121064380?spm=1001.2014.3001.5506#:~:text=9.%20Suppose%20a%20web%20page%20consists%20of%20a%20base%20HTML%20file%2C%205%20JEPG%20images%20and%20a%20java%20applet%2C%20and%20also%20suppose%20HTTP%20uses%20persistent%20connection%20without%20pipelining%2C%20the%20total%20response%20time%20is%EF%BC%88%20%EF%BC%89%20.) 

- on the behalf of 代表

- parallel 并行

- parameter 参数

- push 推 pull 拉

- stateless

  > POP3

- Host aliasing 主机别名

- Mail server aliasing 邮件服务器别名

- Load distribution 负载分配

- overlay network 覆盖网络

- Query Flooding 洪泛查询

- incentive priorities 激励优先

- parallel downloading 并行下载

- raise 增加 reduce 减少 

- recipient’s 收件人的

- phases 状态

- authorization, transaction and update 授权 事务 更新

- highly scalable 高可扩展性

- specifies 指定

- indicates 指出

- session 会话

- distinguish  区分

## Chapter 3 Transport Layer

> [Chapter 3 Transport Layer](https://www.docin.com/p-2440585362.html)

- multiplexing and demultiplexing 多路复用和多路分解
- corrupt 破坏
- cumilative 累加的
- full duplex 全双工
- perceive congestion 感知拥塞
- either A or B A或B
- forwarding and filtering 转发和过滤（链路层交换机）
- swiching and routing 交换和路由（网络层路由器）
- MSS maximum  segment 最大报文段长度
- MTU maximum transmission unit 链路层最大帧长度
- grab 抓取
- employs 使用
- reassembles 重新组装
- finer 出色的
- detection 检测
- correct 更正
- plot 图
- threshold 阈值
- latency 延迟

## chapter 4 

- topology 拓扑
- reassemly 重组

## 知识点

### 端口

- 20 and 21 FTP
- 22 SSH
- 25 SMTP
- 53 DNS
- 80 HTTP
- 443 HTTPS
- 110 POP3

### network architecture

- CS

- P2P

- CB

- network architecture is fixed. 网络架构是固定的。

- network architecture provides a specific set of services to application.

  网络体系结构为应用程序提供了一组特定的服务。

- The network architecture is designed by application developer.

  网络体系结构由应用程序开发人员设计。

### 有无状态

有状态

- FTP
- SMTP

无状态

- http
- POP3

### 各层的服务对象

- 应用层——应用——报文
- 传输层——进程——报文段
- 网络层——端——数据报、分组
- 链路层——各种结点，如：路由器——帧
- 物理层————比特流



### port number

- 范围 0-65535
- 固定 0-1023

### socket 信息

- tcp 目标IP+目标socket+源IP+源socket （4 tuple）
- udp 目标IP+目标socket （2 tuple）
- 一个进程可以对应多个socket，但是一个socket只能对应一个进程
- 若两个udp报文段有不同源，但只要目标相同，都通过同一个socket

### rdt演变

- rdt 1.0
  - 完全可靠信道
- rdt 2.0
  - 具有比特差错，假设ACK和NAK无损
  - 使用ACK和NAK建立自动重传请求ARQ
  - 停等协议：每发送一个分组都要接受端的确认信号
- rdt 2.1
  - 解决rdt 2.0 信号受损
  - 给分组设置序号
- rdt 2.2
  - 只有ACK没有NAK
- rdt 3.0
  - 比特差错和分组丢失
  - 超时重传
- rdt 3.0 改良
  - rdt3.0性能太低
  - 引入流水线协议
    - 差错恢复（GBN\SR）

### TCP

#### 服务

- 提供

1. reliable transport ; 
2. flow control; 
3. congestion control;
4. connection-oriented. 

- 不提供

1. timing、delay guarantee
2. minimum throughput guarantee;
3. security

#### 标志

- SYN 连接建立
- FIN 连接释放

#### 特点

- 点对点，不能多播
- 全双工
- 三次握手
  - 确保客户端活跃
- 四次挥手
  - 因为是全双工

### UDP

- 头部字段
  - source port number
  - destination port number
  - length
  - checksum

### 校验和

1110 0110 0110 0110 ①

1101 0101 0101 0101 ②

1 1011 1011 1011 1011 ①+②

1011 1011 1011 1100 反卷

> 1. 去掉头部的1
>
> 2. 整体再加1

0100 0100 0100 0011 ③校验和结果

验证：①+②+③ = 全1（没出错）

### MSS MTU的关系

MSS的大小根据MTU的大小确认

MSS + TCP\IP首部长度 <= MTU

### 往返时间估计

EST RTT = (1-a) EST RTT + a * Sam RTT

> Sam RTT 一次往返时间

### 接收窗口计算

rwnd = RcvBuffer - （LastByteRcvd - LastByteRead）
