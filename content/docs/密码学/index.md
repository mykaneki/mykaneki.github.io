---
title: "密码学学习笔记"
date: 2022-01-07
draft: false
tags: ["密码学"]
---

# 1. 绪论

> 任何需要可信的第三方的计算，都可以舍弃第三方

## 1.1 Security Objectives 

1. 保密性 Confidentiality

   > 信息的非授权泄露

   1. 数据保密性 Data confidentiality

      > 确保自己的隐私信息不在未授权的情况下透露给他人。

   2. 隐私性 Privacy

      > 确保自己的隐私能够被自己控制，如：向谁授权，让其获得这些隐私。

2. 完整性 Integrity

   > 信息的非授权更改和毁坏

   1. 数据完整性 Data integrity

      > 确保信息和程序只能以特定和授权的方式进行更改，避免信息的不恰当更改或破坏。
      >
      > 包括了信息的不可否认性和真实性。

   2. 系统完整性 System integrity

      > 确保系统能正常的执行预定的功能，避免非授权操纵。

3. 可用性 Availability

   > 确保系统工作讯息，避免信息系统访问和使用的中断，对授权用户不能拒绝服务。

4. 真实性 Authenticity

   > 验证用户是否是他声称的那个人。
   >
   > 验证系统的每个输入是否来自可信任的源。

5. 可追溯性 Accountability

   > 当出现安全事故时，我们必须能够追查到责任人。

## 1.2 OSI安全框架

1. 安全攻击 Security attack

   > 任何危及信息系统安全的行为。

   ![攻击的类型](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209241137590.png) 

   1. 被动攻击

      > 被动攻击不涉及流量的修改，难以检测，因此应该把重点放在预防上。

      ① 信息泄露 The release of message 
      contents

      ② 流量分析 Traffic analysis

      经过加密保护的信息还是具有消息模式的特点，攻击者可以根据传输消息的频率和长度来获取某些信息。

   2. 主动攻击

      > 对数据流进行修改和伪造

      ① 伪装 Masquerade

      伪装别的实体。能够做到没有权限的实体冒充有权限的实体从而获取权限。

      ② 重播 Replay

      源主机或者攻击者将获得的信息再次发送，可达到欺骗系统的目的，主要用于身份认证过程，破坏认证的正确性。

      ③ 消息修改 Data Modification

      修改合法消息的内容、延迟信息传输、更改消息的顺序。

      ④ 拒绝服务 Denial of service

      向服务器发送大量数据，使其崩溃。

2. 安全机制 Security mechanisms 

   1. 加密 Cryptographic algorithms

      ![加密算法的类型](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209242101173.png)

      > 1. 无密钥加密算法 Keyless Algorithms
      >
      >    对一段信息的摘要
      >
      >    ① hash函数
      >
      >    ② 伪随机数
      >
      > 2. 单密钥\对称密钥加密算法 Single-Key \Asymmetric Algorithms
      >
      >    ① 分组\块密码 Block cipher
      >
      >    转换不仅依赖于当前数据块和密钥，而且还依赖于前面块的内容
      >
      >    ② 流密码 Stream cipher
      >
      >    转换依赖于密钥
      >
      >    ③ 消息认证码 Message Authentication Code (MAC)
      >
      >    将通信双方共享的密钥K和消息m作为输入，生成一个关于K和m的函数值MAC，将其作为认证标记(Tag)。发送时，将消息和认证码同时发送给接收方，若接收方用消息和共享密钥生成相同的消息认证码，则认证通过。
      >
      >    ![消息认证码图示](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209242115998.png) 
      >
      >    
      >
      > 3. 双密钥\非对称加密算法 Two-Key\Asymmetric Algorithms
      >
      >    ① 数字签名算法 Digital signature algorithm
      >
      >    ![签名生成流程](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209242149017.jpeg) 
      >
      >    ② 密钥交换 Key exchange
      >
      >    ③ 用户认证 User authentication

   2. 数字签名 Digital signature

   3. 访问控制 Access control

   4. 数据完整性 Data integrity

   5. 认证交换 Authentication exchange

      通过信息交换来保证实体身份

   6. 流量填充 Traffic padding

      在数据流中插入若干位组织流量分析

   7. 路由控制 Routing control

      为某些数据选择安全的物理线路并允许路由变化

   8. 公证 Notarization

3. 安全服务 Security service

   1. 认证 Authentication 

      ① 对等实体认证 Peer entity authentication

      在连接建立和数据传输的阶段，确认连接的双方没有假冒的。

      ② 数据源认证 Data origin authentication

      仅确保数据来源，不提供其他保护机制，如：数据修改。这样的认证常用于电子邮件等应用，通信实体事先并无交互。

   2. 访问控制 Access Control

      阻止对资源的非授权使用，每个实体或用户都对某个资源具有不同的权限，因此在使用资源之前需要认证身份，然后才能授权。

   3. 数据保密性 Data Confidentiality

      防止数据遭到被动攻击 

      > 连接保密性：保护一次连接中所有用户的信息
      >
      > 无连接保密性：保护单个数据块里所有用户的信息
      >
      > 选择域保密性：保护单条信息或单条信息里指定的数据部分
      >
      > 流量保密性：防止流量分析

   4. 数据完整性 Data Integrity

      提供的服务分为有连接和无连接、具有恢复功能和无恢复功能

   5. 不可否认性 Nonrepudiation

   6. 可用性服务 Availability Service

4. 安全服务和机制间的联系

   ![安全服务和机制间的联系](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209242152777.png)

   ## 1.3 网络安全关键因素 

   ![网络安全关键因素](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209242203320.png)

   1. 通信安全

      借助安全协议，如SSH、HTTPS等。

   2. 服务安全

      保护安全设备，如：路由、交换机等。

      几种安全设备：

      ① 防火墙 Firewall

      防火墙作为一个过滤器，基于一组基于流量内容和/或流量模式的规则，允许或拒绝数据传入和传出流量。

      ② 入侵检测 Intrusion detection

      发出警报但不做出制止行为

      ③ 入侵防护 Intrusion prevention

      在入侵前检测并拦截入侵行为



# 2. 传统加密技术

## 2.1 对称密码模型

- 明文 Messege\X 
- 加密算法 Encrypt
- 密钥 Key
- 密文 Cyphertext\Y 
- 解密算法 Decrypt

$$
c=E(k,m) \\
  m=D(k,c)\\
  D(k,E(k,m))=m\\
$$

### 2.1.1 密码编码学

  密码编码学系统有以下3个独立的特征：

1. 转换明文密文的运算类型

   代替和置换

2. 所用的密钥数

3. 处理明文的方法

### 2.1.2 密码分析学和穷举攻击

- 密码分析学

  > 唯密文攻击、已知明文攻击、选择明文攻击、选择密文攻击、选择文本攻击

  - 无条件安全

    无论有多少可使用的密文，都不足以唯一地确定密文所对应的明文，则称该加密体制是**无条件安全**的。

  - 计算上安全的算法的标准

    ① 破译密码的代价超过密文信息的价值；

    ② 破译密码的时间超过密码的有效生命期。

- 穷举攻击

## 2.2 代替技术

### 2.2.1 Caesar密码

- 字母表移位
- 密钥空间25
- 明文易于识别

### 2.2.2 单表代替密码

- 密文行是26个字母的任意替换
- 密钥空间 26! 
- 明文可以被字频分析，词频破解

### 2.2.3 多字母代替算法 Playfair密码

- 以一个密钥词（monarchy）以及剩余字母，填充矩阵格子，每两个字母加密一次，在相同的字母之间加一个填充字母。

  | M    | O    | N    | A    | R    |
  | ---- | ---- | ---- | ---- | ---- |
  | C    | H    | Y    | B    | D    |
  | E    | F    | G    | I/J  | K    |
  | L    | P    | Q    | S    | T    |
  | U    | V    | W    | X    | Z    |

- 密钥空间26*26=676

- 词频破解

### 2.2.4 多表代替密码 HIll密码

### 2.2.5 多表代替加密

- Vigenere 密码

  - 密钥是一个密钥词的重复

  | 明文 | H    | e    | l    | l    | o    | w    | o    | r    | l    | d    |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | 密钥 | a    | b    | c    | a    | b    | c    | a    | b    | c    | a    |
  | 密文 | H    | f    | n    | l    | p    | y    | o    | s    | n    | d    |

  ![img](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209251343709.png)

  - 即使有如此算力能够穷举出来也没办法根据明文信息判断哪一句是真正的明文，因为会有很多句都有具体的意义

  - 发现重复的密文序列，可以推算出密钥长度

- Vernam 密码

  - 基于二进制数据而非字母，使用异或加密
  - 使用周期很大的循环密钥
  - 如果有足够的密文，使用已知明文序列，可以被破解

### 2.2.6 一次一密

- 一次性密码本，明文长度和密钥一样长
- 该方法完全安全
- 密码太长，难以保护

## 2.3 置换技术

- 加密方法

  1. 按对角线的顺序写出明文（栅栏技术）

     明文：meet me

     密文：

     ```c
     m	e	m
     e	t	e
     ```

  2. 将消息一行一行地写成矩形块，把列的顺序打乱，列的次序就是密钥（置换密码）

     > 一次加密可以很容易单个字母词频攻击，可以多次加密，这样分析难度大得多

     | 密钥 | 2    | 1    | 3    |
     | ---- | ---- | ---- | ---- |
     | 明文 | M    | E    | E    |
     |      | T    | M    | E    |
     | 密文 | EM   | MT   | EE   |

## 2.4 转轮机

![三轴转轮机](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1431/202209260937121.png)

使用n个轮子密钥长度就是替换过n次的Vigenere密码
$$
26^n
$$

## 2.5 隐写术

应用于通信双方宁愿丢失，也不愿他们进行密码通信的事实被人发现





1. 密码学的三个步骤
   1. 精确指定威胁模型，例如：攻击者如何攻击数字签名，伪造签名的目的何在？
   2. 提出架构。证明任何攻击者在这个威胁模型下可以攻击这个架构的这个攻击者也可以用来解决根本性难题。
   3. 如果能够在威胁模型下攻击该架构，那么他可以解决世纪难题。进而反推他无法成功攻击该架构。
2. 发展史
   1. 替换式密码
      1. 凯撒加密（词频破解、字母配对频率）
      2. vigener （用一个单词作为密钥）
      3. 滚轮机器（用碟片）
   2. 





# 英语单词

1. Security Objectives 网络安全的目标