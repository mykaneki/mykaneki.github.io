---
title: "区块链笔记"
date: 2023-04-20
tags: [‘区块链‘]

---

## 相关资源 

bilibili：[1-7：签名交易_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Ca411n7ta/?p=10&spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=694b89c946544771345ce2b3283d9b35) 

GitHub：https://github.com/smartcontractkit/full-blockchain-solidity-course-js

FISCO BCOS：[关键概念 — FISCO BCOS v2.9.0 文档 (fisco-bcos-documentation.readthedocs.io)](https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/docs/tutorial/key_concepts.html) 

## 第 1 课：区块链基础知识

### 什么是区块链？区块链有什么作用？

以密码学为基础，让人们以去中心化的方式，发生非许可的金融交易行为

区块链（blockchain）是在比特币之后提出的一个概念，在中本聪关于比特币的[论文](https://bitcoin.org/bitcoin.pdf)中没有直接引入blockchain的概念，而是以chain of block来描述一种数据结构。

Chain of block是指由多个区块通过哈希（hash）串联成一条链式结构的数据组织方式。区块链则是采用多项技术交叉组合，维护管理这个chain of block数据结构，形成一个不可篡改的分布式账本的综合技术领域。

比特币白皮书 —— 中本聪（匿名）—— 总量有限，（类似于黄金的）电子价值存储

以太坊 —— 除了价值存储，还是运行去中心化合约的平台（签署去中心化的合同、构建去中心化的组织等）

智能合约 —— 去中心化的合同（绝对不可违背、不可篡改的约定）

混合智能合约 —— 将链上运行的代码和链下去中心化预言机获取的数据和计算相结合

预言机 —— 为区块链提供真实世界的数据和计算

web1 —— 有着固定内容的非许可的开源网络

web2 —— 网络内容动态的许可（中心化）网络

web3 —— 网络内容动态且非许可（去中心化）网络

DeFi —— 去中心化金融，用户可以参与到金融市场而不需要经过中心化的中介

DAOs —— 去中心化自治组织，它们被区块链上的一组指令或者一个智能合约管理

NFT —— 非同质化代币，它在某种程度是一种电子艺术品，或者一个独一无二的资产（无聊猿和加密朋克）

### 第一笔交易

创建钱包 —— https://metamask.io/ —— 浏览器插件 

助记词 —— 不可泄露、不可遗忘 —— 获得助记词可以打开所有账户

账户 —— 一个钱包可以有多个账户，每个账户对应一个私钥（不可泄露）

以太坊主网 —— 真实的交易网

测试网 —— 测试网有很多个，上面的货币都是假的，可以请求水龙头（也有很多个）发送假的代币

水龙头推荐 —— https://github.com/smartcontractkit/full-blockchain-solidity-course-js#testnet-faucets 

区块浏览器 —— https://etherscan.io/ —— 用它来查看不同的交易，地址

测试网的相应浏览器 —— https://sepolia.etherscan.io/ ——谷歌搜索Sepolia etherscan（一般加个前缀）

![image-20230420112814933](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304201128980.png)

![image-20230420112438498](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304201124558.png)

![image-20230420112130551](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304201121712.png)

![image-20230420112305146](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304201123229.png)



![image-20230420112327460](https://raw.githubusercontent.com/mykaneki/picgo/master/img/202304201123586.png)

认识浏览器中的信息：

1. **Transaction Hash**

   交易哈希：在这个区块链上这笔交易的唯一 ID

2. Method 

3. **Block**

   区块高度

4. Age

5. **From**

6. **To**

7. **Value**

8. Txn Fee

9. **Gas Price** 

   交易手续费和 gas 价格：付给矿工处理这笔交易的费用

   gas 价格是交易中每个执行单元的花费(用ether和gwei做单位)

   gas 价格越高，被写到区块中的机会越大

### 区块链如何运作？

哈希 挖矿（随机数） 解决难题（哈希值以0000开头） 

创世区块（prev指针为0） 区块链（数据结构链） 分布式 （ABC三方会互相监督）

https://andersbrownworth.com/blockchain/

### 证明交易发起人

公私钥验证 

私钥生成公钥 公钥是公开地址的主要组成部分

### 共识和常见攻击

共识的定义是一个机制，通过这个机制，区块链可以在状态和数值上达成一致

中本聪共识（以太坊1.0和比特币）包括了工作量证明和最长链规则（哪条链有最长，有最多的区块，就用哪条链）

工作量证明和权益证明都属于共识

谁得到了**交易手续费**？是矿工或者叫验证者

**女巫攻击**：在攻击中，用户会创建很多匿名账户

抗女巫机制：指一种区块链的能力，来防止用户使用大量的假身份

**51% 攻击**：有最长的链和 51% 的网络，那就可以分叉区块链，让整个网络使用你的链（如果一些节点有足够的算力，它实际上就是  51% 的网络，从而影响区块链在它的方向上前进，这就是 51% 攻击）

工作量证明：全部节点一起比赛挖矿，先挖到的获得奖励

> 问题：不环保

权益证明（比特币2.0）：权益证明需要放置一些抵押物以保证不作恶，全部节点协商出一个节点进行挖矿，其他进行验证

> 权益证明的网络，被认为有一些不够去中心化

**链选择的规则**：怎样确定哪个区块链是正确的链

**确认区块的数量**：在我们的交易写入区块后，新挖出的区块数量，如果看到确认区块是 2，就表示在最长的链中，我们交易后面有 2 个区块

**可扩展性**

> 在讲 gas price 时提到过，如果同时有很多人发送交易，gas price 会非常高，因为区块的存储空间有限，节点存储的交易也有限，所有当很多人想要使用区块链的时候，gas price 就会暴涨，这就无法实现**扩展**性，因为我们想让更多的进入区块链

**分片**就是**可扩展性**问题的一种解决方案

有一个主链会协调一些不同的链，将他们连接在一起，这意味着，人们可以在多个链上发送交易，很有效率的提高了区块链的空间，分片可以极大的增加在 layer1 上发送的交易数量。

**layer 1 和 layer 2**

layer 1 是区块链实现的基础层，比特币，以太坊和 avalanche 都是 layer 1

layer 2 是加在 layer 1 和区块链上的任何应用，比如，Chainlink，Arbitrum 和 Optimism 都是 layer 2

**Rollup** 是 layer 1 的可扩展性问题的解决方案（Arbitrum 和 Optimism）

