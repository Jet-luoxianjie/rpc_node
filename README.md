---
title: rpc解析和手写rpc框架（芜湖起飞🚀）
renderNumberedHeading: true
grammar_cjkRuby: true
---


<!-- TOC -->

- [[1] 为什么要用rpc？](#为什么要用rpc)
- [[2] 什么是rpc？](#什么是rpc)
- [[3] 实现远程调用的一些思路？](#实现远程调用的一些思路)
- [[4] 从0到1实现简易RPC框架](#从0到1实现简易RPC框架)
- [[5] 谷歌GRPC框架解析](#谷歌GRPC框架解析)
<!-- /TOC -->

## 为什么要用rpc

 当项目越来越大时，**集中式**的服务(如:单一**game**服)缺点越来越明显。当然应对的方案是对项目的**拆分**(分为**logic,team,battle**等),分对多个独立的服务来开发，后随着网路请求越来越大，多个独立的服务也需要**独立部署**到**不同的物理机**上。
 
>**这里就出现了一个问题**
> ---------
> 如果team服要实现一个功能，数据只存在logic服,而logic服也有对应接口。team服怎么办？
> 
> #### 方法1：把logic的数据传输到team服，team服再写对应的函数。
> **优点**：
> 1.传输一次后，后续调用数据可以在进程内调用，快捷方便。
> **缺点**：
> 1.**logic**和**team**服都维护一样数据，容易出现数据不一致。
> **总结**：**该方法适用于数据不易变的情况。**
> 
> #### 方法2：team服发送一个消息到logic服，然后等待logic服返回消息在往下运行。
> **优点**：
> 1.调用方便。（框架实现的好类似调用本地函数一样）
> **缺点**：
> 1.每次都要调用都是一次请求，相对于本地调用会有额外网络开销会影响。
> **总结**：**适用于数据易变的情况。**
> #### 关于方法2，有一个RPC协议刚刚就可以解决这个问题。

## 什么是rpc

RPC 是 1984 年代由 Andrew D. Birrell & Bruce Jay Nelson 提出的，所以并不是最近的概念，在二位大神的论文 "Implementing Remote Procedure Calls"。 (实现远程过程调用)

> **论文连接** : https://pages.cs.wisc.edu/~sschang/OS-Qual/distOS/RPC.htm

**论文简单概括**：就是让分布式系统更加简单，让开发人员把精力放到业务上，并且提供高效安全的通信。


**再来看看比较常见的解释，了解下 RPC 是啥：**

> RPC(Remote Procedure Call) 远程过程调用，它是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。也就是说两台服务器 A，B，一个应用部署在 A 服务器上，想要调用 B 服务器上应用提供的方法，由于不在一个内存空间，不能直接调用，需要通过网络来表达调用的语义和传达调用的数据。

**大白话理解这段话就是说**：RPC 让你调用其他服务器的函数和调用本地函数一样。

## 实现远程调用的一些思路

   前面说了 RPC 远程过程调用就是让服务 A 像调用本地功能一样调用远端的服务 B 上的功能，不要把这个事情想的太悬乎。
  **想想我们本地调用时需要哪些东西:**
  **1.类或函数
  2.类或函数的参数
  3.类或函数的返回值。**

![](https://github.com/Jet-luoxianjie/rpc_node/blob/main/images/dy.png?raw=true)

远程调用肯定也不会缺少这三要素，唯一的区别在于这三要素是要被传输过去的，这其中就涉及协议编码和解码的过程。

**远程调用时**
这样服务 team需要通过网络传输来告诉服务 logic，它想要获取玩家信息，传入的两个参数为 1，返回的结果放在 **playerInfo** 里面就可以。
![](https://github.com/Jet-luoxianjie/rpc_node/blob/main/images/dy1.png?raw=true)

传输的报文里面按照约定的协议格式给出了**函数名**和**参数**和**返回值**即可。


> **看到网上会有人问 tcp,http,tcp的区别？（因为http也可以请求呀）**
>   rpc是一个远程调用的协议，而http是超文本传输协议，tcp是传输控制协议。
>   rpc的框架是对 http ，tcp等协议进行的封装和优化，来实现一个**远程调用**,rpc**重点**是**远程调用**，而不是协议。


## 从0到1实现简易RPC框架

**当前简易RPC 的目的是以最少的代码，实现 RPC 框架中最为重要的部分，帮助大家理解 RPC 框架在设计时需要考虑什么。代码简洁是第一位的，功能是第二位的。**

//todo:

## 谷歌GRPC框架解析

//todo:
