---
layout: post
title: TCP的连接建立与关闭
---

{{ page.title }}
=========

@[tcp|connection|state]

### TCP正常连接建立与关闭 ###
##### TCP建立连接(3次握手) ####
1. server被动打开, 状态CLOSED -> LISTEN
2. client主动打开, 发送SYN, 状态CLOSED -> SYN_SENT
3. server收到SYN, 发送SYN+ACK, 状态LISTEN -> SYN_RCVD
4. client收到SYN+ACK, 发送ACK, 状态SYN_SENT -> ESTABLISHED
5. server收到ACK, 状态SYN_RCVD -> ESTABLISHED

    > 3次握手分别对应 2->3 3->4 4->5

##### TCP关闭连接(4次握手) #####
1. client主动关闭, 发送FIN, 状态ESTABLISHED -> FIN_WAIT_1
2. server收到FIN, 发送ACK, 状态ESTABLISHED -> CLOSE_WAIT
3. client收到ACK, 状态FIN_WAIT_1 -> FIN_WAIT_2
4. server被动关闭, 发送FIN, 状态CLOSE_WAIT -> LAST_ACK
5. client收到FIN, 发送ACK, 状态FIN_WAIT_2 -> TIME_WAIT
6. server收到ACK, 状态LAST_ACK -> CLOSED
7. client等待2MSL超时, 状态TIME_WAIT -> CLOSED

    > 4次握手分别对应 1->2 2->3 4->5 5->6
    >
    > MSL Maximum Segment Lifetime
