---
layout: post
title: TIME_WAIT and CLOSE_WAIT
category: network
tags: [network, tcp/ip, linux, time_wait, close_wait]
---

# {{ page.title }} #

## TIME_WAIT ##

TIME_WAIT发生在主动关闭的一方。主动关闭的一方在发送ACK响应被动关闭的一方发出的FIN后，状态变为TIME_WAIT，再等待2MSL时长后，关闭连接。

解决TIME_WAIT太多的方法

> * 设置重用TIME_WAIT连接`net.ipv4.tcp_tw_reuse`，注意是被连接一方，不存在复用一说。也就是说如果是服务器端主动关闭连接，在服务器端无法复用TIME_WAIT
> * 设置快速回收TIME_WAIT连接`net.ipv4.tcp_tw_recycle`，不推荐使用，当`net.ipv4.tcp_timestamps`也开启时，在NAT网络环境下，由于时间戳错乱，会出现丢包的情况
> * 控制TIME_WAIT的总数`net.ipv4.tcp_max_tw_buckets`
> * 服务端使用keepalive，由客户端主动关闭连接

## CLOSE_WAIT ##

CLOSE_WAIT发生在被动关闭的一方。被动关闭的一方在发送ACK响应主动关闭的一方发出的FIN后，会返回给应用程序一个结束符，等待程序关闭连接。

## 参考 ##

1. TCP/IP 详解 卷1: 协议
2. [火丁笔记](http://huoding.com/ "火丁笔记")
