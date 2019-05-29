---
title: master-tcp
date: 2019-05-22 09:18:08
tags: ["tcp"]
---
> The Transmission Control Protocol (TCP) is intended for use as a highly
reliable **host-to-host** protocol between hosts in **packet-switched** computer communication networks, and in interconnected systems of such networks.
# TCP header format
![tcp header](/images/master-tcp-tcp-header.png)
## basic data transfer

## reliability
* sequence number
* acknowledgement

## flow control
* window size

## multiplexing
* socket (ip, port)

## connections
> Each connection is uniquely specified by a **pair** of sockets identifying its two sides.(source ip, source port),(destination ip, destination port)

* sockets
* sequence numbers
* window size

# TCP 建立连接目的
> 交换通信双方的 ISN (Initial Sequence Number)
> 凡是占据序列号的任何TCP报文，一定对方确认，如果没有收到确认，会一直重传，直到达到规定的上限次数。
> syn , data , fin 占据 sequence number 需要确认. ack 不占据 sequence number 不需要确认.
















# 参考链接
1. [TCP three way handshake 1](https://mp.weixin.qq.com/s?__biz=MzA3MDMwOTcwMg==&mid=2650005578&idx=1&sn=9e4ba700512e68e2dcbd54bfe11bd669&chksm=87398263b04e0b7577be99e43c272147b74030514df3f78b894c5b5cee21a3565cf3e4ceebeb&scene=21#wechat_redirect)
2. [TCP three way handshake 2](https://mp.weixin.qq.com/s?__biz=MzA3MDMwOTcwMg==&mid=2650005835&idx=1&sn=dd0104635b5510ab6bf4f34ec347fe57&chksm=87398362b04e0a7402a6075b46e9a62855a78408a6dfecf6f7d13d8ae5e0375fab6d84208159&scene=21#wechat_redirect)
3. [rfc793](https://tools.ietf.org/html/rfc793)
