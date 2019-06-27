---
title: "centos install mysql (no network)"
tags: ["mysql","jlch"]
categories: ["mysql"]
date: "2018-01-31T12:26:38+08:00"
subtitle: "no network"
draft: false
---

## env

需要在 centos 环境下安装 mysql

192.168.31.175

## step

下载[mysql的rpm安装包](https://dev.mysql.com/downloads/file/?id=474693)

```
[root@localhost jlch]# rpm -ivh mysql-community-server-5.7.21-1.el7.x86_64.rpm
warning: mysql-community-server-5.7.21-1.el7.x86_64.rpm: Header V3 DSA/SHA1 Signature, key ID 5072e1f5: NOKEY
error: Failed dependencies:
	/usr/bin/perl is needed by mysql-community-server-5.7.21-1.el7.x86_64
	mysql-community-client(x86-64) >= 5.7.9 is needed by mysql-community-server-5.7.21-1.el7.x86_64
	mysql-community-common(x86-64) = 5.7.21-1.el7 is needed by mysql-community-server-5.7.21-1.el7.x86_64
	net-tools is needed by mysql-community-server-5.7.21-1.el7.x86_64
	perl(Getopt::Long) is needed by mysql-community-server-5.7.21-1.el7.x86_64
	perl(strict) is needed by mysql-community-server-5.7.21-1.el7.x86_64
[root@localhost jlch]#
```

要安装 `perl`, [下载](http://mirror.centos.org/centos/7/os/x86_64/Packages/perl-5.16.3-292.el7.x86_64.rpm)

```
tar -zxvf perl-5.26.1.tar.gz
cd perl-5.26.1
./Configure -de
``` 

报错, 没有`cc`,`gcc`
