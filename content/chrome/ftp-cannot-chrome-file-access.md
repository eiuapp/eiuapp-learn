---
title: "Ftp Cannot Chrome File Access"
date: 2018-05-29T15:11:41+08:00
draft: false
tags: ["ftp", "centos"]
categories: ["ftp"]
subtitle: "为何客户端软件可以而 浏览器和windows文件夹 中ftp:// 则不能连接FTP服务器"
descriptions: "为何客户端软件可以而浏览器则不能连接FTP服务器"
bigimg:
---

为何客户端软件可以而浏览器则不能连接FTP服务器

## env

- centos6.9
- ftp: vsftpd

## step

这里主要的点就是理解主动模式与被动模式。

主动模式对服务器端有利，被动模式对客户端有利。但是服务器端既然要提供FTP服务，应该在服务器端设置防火墙规则，提供被动模式服务，而不能要求所有的客户端都设置防火墙规则以适应主动服务模式。当然服务器端在提供被动模式服务时，出于安全考虑，应该设置客户端可发起数据连接的端口范围，这就是下面配置文件中的

```
pasv_min_port=61001
pasv_max_port=62000
```
即，告诉客户端可以向61001与62000之间的端口发起数据连接。

```
[root@bitspace ~]# tail /etc/vsftpd/vsftpd.conf 
# sockets, you must run two copies of vsftpd with two configuration files.
# Make sure, that one of the listen options is commented !!
#listen_ipv6=YES

pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES

pasv_min_port=61001
pasv_max_port=62000
[root@bitspace ~]# 
```
## Ref

- https://blog.csdn.net/zilong00007/article/details/7663152
- https://my.oschina.net/iyinghui/blog/1605801
- http://www.cnblogs.com/qytan36/archive/2010/05/15/1736270.html