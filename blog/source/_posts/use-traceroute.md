---
title: use-traceroute
tags: [icmp, traceroute]
date: 2018-10-08 20:29:16
categories: 网原
---

### ICMP

- [x] ICMP在Internet的哪一层呢？在网络层中。

ICMP基于IP协议，被`ping`和`traceroute`使用，定义在RFC 792中。ICMP被设计来完成的目标是判断网络中其他主机是否活跃以及获得它们的基本信息。

每个ICMP包都被封装在一个IP包中。

ICMP协议中没有端口，但是有另外两个域：`type`和`code`。

- type：生成的错误报文的类型。
- code：用来查找错误的原因

`type==0&&code==0`表示一个Echo Reply，被`ping`利用（表示ping成功的信息）。

### TraceRoute

TTL存在于IP包头中，有8bit，最大能设为255，推荐设的初始值为64。在ICMP中，如果TTL为0，当前主机会向源主机发送TTL超时的信息，从而源主机可以获得当前主机的地址。

> 由于要求沿途的路由器在转发数据包前至少必须将 TTL 减少 1，因此 TTL 实际上是一个跃点计数器 (hop counter)。当某个数据包的 TTL 达到零 (0) 时，路由器就会向源计算机发送一个 ICMP“超时”的消息。

> TRACERT 将发送 TTL 为 1 的第一个回显数据包，并在每次后续传输时将 TTL 增加 1，直到目标地址响应或达到 TTL 的最大值。中间路由器发送回来的 ICMP“超时”消息显示了路由。请注意，有些路由器会丢弃 TTL 失效的数据包而不发出消息，这些数据包对于 TRACERT 来说是不可见的。

### Windows下使用`tracert`

#### 基本用法

`tracert <hostname>	`

在Windows上，可以看见每一跳的往返时间。对于同一个TTL，tracert发送了三个包。对于每一跳返回的ip地址，tracert会尝试使用DNS解析出它的域名方便我们查看（用`-d`选项可以不执行这一步）。

如果某个往返时间显示为"*"，说明ICMP包超时丢失。可能是因为目标主机为了防止网络攻击，丢弃了ICMP包。用`-w timeout`可以设置等待超时的毫秒数。

- [ ] 为什么有的时候TTL值更大的主机往返时间更短呢？

## 参考

- [ICMP与IP之间的关系](http://blog.sina.com.cn/s/blog_141bede520102wt00.html)
- [简单了解ICMP协议](https://blog.csdn.net/xionghuixionghui/article/details/70489246)
- https://zh.wikipedia.org/wiki/%E4%BA%92%E8%81%94%E7%BD%91%E6%8E%A7%E5%88%B6%E6%B6%88%E6%81%AF%E5%8D%8F%E8%AE%AE
- https://support.microsoft.com/zh-cn/help/314868/how-to-use-tracert-to-troubleshoot-tcp-ip-problems-in-windows
- https://zhuanlan.zhihu.com/p/27210571