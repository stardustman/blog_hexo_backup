---
title: master-ip
date: 2019-06-04 15:32:28
tags: ["network"]
---
# Motivation
  The Internet Protocol is designed for use in interconnected systems of **packet-switched** computer communication networks.  Such a system has been called a "catenet".  The internet protocol provides for transmitting blocks of data called datagrams from sources to destinations, where sources and destinations are hosts identified by **fixed length addresses**.  The internet protocol also provides for **fragmentation** and **reassembly** of long datagrams, if necessary, for transmission through "small packet" networks.
# IP format
  {% asset_img ip-header-format.png ip-format %}

# IPV4 example 
  {% asset_img ip-capture-wireshark.jpg ip-capture-wireshark %}
  {% asset_img ip-capture-wireshark-example.png ip-capture-wireshark-example %}
## version(4 bits)
0x4(4)
## Header Length(4 bits)
0x5(5)
## Type Of Service(TOS)(8 bits)
0x00(0)
## Total Length(16 bits)
0x28(40)
## Identification(16 bits)
0xd279
## Flags(3 bits)
0x4 0100
1. 0 Reserved bit not set
2. 1 Don't fragment set
3. 0 More fragments not set

## Fragment Offset(13 bits)
> This field indicates where in the datagram this fragment belongs.
> The fragment offset is measured in units of 8 octets (64 bits). The
first fragment has offset zero.

0x000 (0000 0000 0000)
## Time to Live(TTL)(8 bits)
0x80 TTL:(128)
## Protocal(8 bits)
0x06 TCP:(6)
## Header Checksum(16 bits)
0x2e35
## Source IP Address(32 bits)
0x 0a:fe:c9:ad -> 10.254.201.173
## Destination IP Address(32 bits)
0x ac:10:79:65 -> 172.16.121.101


  
