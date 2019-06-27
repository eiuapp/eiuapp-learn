---
title: "Ip Config Centos"
date: 2018-02-03T14:26:37+08:00
draft: true
tags: ["ip"]
categories: ["ip"]
subtitle: ""
descriptions: ""
bigimg:
---

centos 下 配置 IP

```
[cdn@test_240 network-scripts]$ cat ifcfg-em1
TYPE=Ethernet
BOOTPROTO=none
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_FAILURE_FATAL=no
NAME=em1
UUID=eccda91b-d9a5-4516-b98f-e1ae014b2f6f
DEVICE=em1
ONBOOT=yes
IPADDR=192.168.31.240
PREFIX=24
#GATEWAY=192.168.31.1
IPV6_PEERDNS=yes
IPV6_PEERROUTES=no

[cdn@test_240 network-scripts]$ cat ifcfg-em2
TYPE=Ethernet
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME=em2
UUID=8e4cae4f-034a-4f69-860f-1f05e07cd350
DEVICE=em2
ONBOOT=yes
DNS1=192.168.3.1
IPADDR=192.168.3.240
PREFIX=24
GATEWAY=192.168.3.1
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
```