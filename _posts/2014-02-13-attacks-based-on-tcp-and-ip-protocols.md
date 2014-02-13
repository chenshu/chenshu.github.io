---
layout: post
title: 基于tcp/ip协议的网络攻击
category: network
tags: [network, tcp/ip, protocols, attack]
---

{{ page.title }}
=========


SYN Flood
---

client伪造ip发送SYN给server来建立TCP连接，server的状态变为SYN_RCVD并把响应SYN+ACK给client。由于client的ip根本不存在，server会不断超时重试。内核维护了一个这种没有收到client的ACK确认的连接队列，称为半连接队列。当半连接队列满了以后，内核会忽略其他建立连接的请求。

攻击者可以通过伪造ip发起大量的TCP连接，直到被攻击者的TCP连接资源全部耗尽。

解决方法:

* 适当的调整半连接队列的长度`net.ipv4.tcp_max_syn_backlog`，启用syn cookie后，这个值将被忽略
* 适当的调整SYN+ACK的重试次数`net.ipv4.tcp_synack_retries`
* 启用连接重置`tcp_abort_on_overflow`，确认是因为攻击而无法建立连接时启用
* 启用syn cookie`net.ipv4.tcp_syncookies`，违反TCP协议，不推荐使用


Attacks based on TCP persist timezr
---

TCP通过让接收方指明希望从发送方接收到的数据字节数(即窗口大小)来进行流量控制。如果窗口大小为0时，将有效地阻止发送方传送数据，直到窗口变为非0为止。

TCP必须能够处理调整窗口大小的ACK丢失的情况。ACK的传输并不可靠，也就是说，TCP不对ACK报文进行确认，TCP只确认那些包含有数据的ACK报文段。

如果一个ACK丢失了，则双方就有可能因为等待对方而使连接终止：

* 接收方等待接收数据(因为它已经向发送方发送了一个非0的窗口调整ACK)
* 发送方在等待允许它继续发送数据的窗口更新

为了防止这种死锁情况的发生，发送方使用一个坚持定时器(persist timer)来周期性向接收方查询，以便发现窗口是否已增大。这些从发送方发出的报文段称为窗口探查(window probe)。

坚持定时器采用指数补偿算法来发送窗口探查，探查间隔会成指数增长，这个过程将持续到窗口被打开，或者应用程序使用的连接被终止。探查重复的次数由`/proc/sys/net/ipv4/tcp_retries2`来控制。

攻击者完成3次握手后，向被攻击者发送一个数据请求，当被攻击者进行响应时，攻击者再向被攻击者发送一个窗口大小调整为0的ACK，这样被攻击者只能等待窗口大小调整的ACK，并且不断的发送窗口探测，直到被攻击者的TCP连接资源全部耗尽。

解决方法:

* 适当的窗口探测的重试次数`net.ipv4.tcp_retries2`


HTTP Slow Header
---


HTTP Slow Post
---


参考
---

1. TCP/IP 详解 卷1: 协议
2. [CUDev](http://blog.chinaunix.net/uid/20357359.html "CUDev")
3. [飘零的代码 piao2010 ’s blog](http://www.piao2010.com/ "飘零的代码 piao2010 ’s blog")
4. [火丁笔记](http://huoding.com/ "火丁笔记")
