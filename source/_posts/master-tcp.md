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
















# 参考链接
1. [为何TCP三次握手的第三个报文不需要确认?](https://mp.weixin.qq.com/s/cS3f6m9zDSdA2Bt2Sfcl5g)
2. [TCP three way handshake 1](https://mp.weixin.qq.com/s/zRelB6uSz07YaCoJoggZZA)
3. [TCP three way handshake 2](https://mp.weixin.qq.com/s?__biz=MzA3MDMwOTcwMg==&mid=2650005835&idx=1&sn=dd0104635b5510ab6bf4f34ec347fe57&chksm=87398362b04e0a7402a6075b46e9a62855a78408a6dfecf6f7d13d8ae5e0375fab6d84208159&scene=21#wechat_redirect)
4. [rfc793](https://tools.ietf.org/html/rfc793)