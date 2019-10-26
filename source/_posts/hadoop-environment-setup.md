---
title: hadoop-environment-setup
date: 2019-07-22 10:40:05
tags: ["hadoop"]
---
# 开发环境
1. CentOS-7-x86_64-Minimal-1810
2. hadoop-3.2.0
3. jdk8
4. vmware 链接克隆 5 台虚拟机
5. 192.168.126.133 s101
6. 192.168.126.134 s102
7. 192.168.126.135 s103
8. 192.168.126.136 s104
9. 192.168.126.137 s105

## 环境配置
### centos7-minimal 配置静态 ip
1. vim /etc/sysconfig/network-scripts/ifcfg-ens33
2. 静态ip配置
```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static #need
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=f5fd61b8-7255-4de9-ae68-5047e1586589
DEVICE=ens33
ONBOOT=yes
NM_CONTROLLED=no #need
IPADDR=192.168.126.137 #need
NETMASK=255.255.255.0 #need
GATEWAY=192.168.126.2 #need
```

### JDK
1. vim /etc/profile 添加
```
export JAVA_HOME=/soft/java8
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/jre/lib:$JAVA_HOME/lib:$JAVA_HOME/lib/tools.jar
```
2. source /etc/profile
3. java -version

### Hadoop
1. vim /etc/profile 添加
```
export HADOOP_HOME=/soft/hadoop-3.2.0
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```
2. source /etc/profile
3. hadoop version
## 



















# References
1. [centos7-static-ip-config](http://www.mustbegeek.com/configure-static-ip-address-in-centos/)
2. [centos7-openjdk-install](https://www.liquidweb.com/kb/install-java-8-on-centos-7/)
3. [centos7-minimal-network-enable](https://blog.csdn.net/crxmai/article/details/49767673)
