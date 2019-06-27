---
title: "Route"
date: 2018-02-23T12:25:05+08:00
draft: false
tags: ["route", "linux"]
categories: ["linux"]
subtitle: ""
descriptions: ""
bigimg:
---

## env

192.168.31.181 上 加一条 wifi 192.168.33.0/24 的路由规则

## ref

[Linux下route add 命令加入路由列表](https://www.cnblogs.com/gccbuaa/p/7117029.html)
## step

```
[root@check ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: em1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000
    link/ether 44:a8:42:2b:e5:e3 brd ff:ff:ff:ff:ff:ff
    inet 192.168.31.181/24 brd 192.168.31.255 scope global em1
       valid_lft forever preferred_lft forever
    inet6 fe80::46a8:42ff:fe2b:e5e3/64 scope link
       valid_lft forever preferred_lft forever
3: em2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP qlen 1000
    link/ether 44:a8:42:2b:e5:e4 brd ff:ff:ff:ff:ff:ff
    inet 10.10.15.181/24 brd 10.10.15.255 scope global em2
       valid_lft forever preferred_lft forever
    inet6 fe80::46a8:42ff:fe2b:e5e4/64 scope link
       valid_lft forever preferred_lft forever
4: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN
    link/ether 52:54:00:83:1d:10 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
       valid_lft forever preferred_lft forever
5: virbr0-nic: <BROADCAST,MULTICAST> mtu 1500 qdisc pfifo_fast master virbr0 state DOWN qlen 500
    link/ether 52:54:00:83:1d:10 brd ff:ff:ff:ff:ff:ff
6: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN
    link/ether 02:42:38:b7:35:73 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 scope global docker0
       valid_lft forever preferred_lft forever
[root@check ~]# route add -net 192.168.33.0 gw 192.168.31.1 netmask 255.255.255.0 em1
[jlch@check ~]$ route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         10.10.15.1      0.0.0.0         UG    100    0        0 em2
10.10.15.0      0.0.0.0         255.255.255.0   U     100    0        0 em2
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
192.168.31.0    0.0.0.0         255.255.255.0   U     100    0        0 em1
192.168.33.0    192.168.31.1    255.255.255.0   UG    0      0        0 em1
192.168.33.0    0.0.0.0         255.255.255.0   U     0      0        0 em1
192.168.122.0   0.0.0.0         255.255.255.0   U     0      0        0 virbr0
[jlch@check ~]$
```


这样, 所有 从 192.168.33.0/24 访问 192.168.31.181 的, 都从 em1 口进出.

上面这个, 当网络重启时,会失效,要手工再加一次,所以, 我们把它写到文件里, 重启一下网络.

```
[jlch@test_240 network-scripts]$ cat /etc/sysconfig/network-scripts/route-em1
192.168.33.0/24 via 192.168.31.1
[jlch@test_240 network-scripts]$ sudo systemctl restart network
```

删除路由规则

```
[root@check ~]# route del -net 192.168.33.0 netmask 255.255.255.0 em1
[root@check ~]#
```
