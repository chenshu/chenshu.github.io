---
layout: post
title: 基于tcp/ip协议的网络攻击
category: network
tags: [network, tcp/ip, protocols, attack]
---

{{ page.title }}
=========

Attacks based on TCP persist timer
---

TCP通过让接收方指明希望从发送方接收到的数据字节数(即窗口大小)来进行流量控制。如果窗口大小为0时，将有效地阻止发送方传送数据，直到窗口变为非0为止。

