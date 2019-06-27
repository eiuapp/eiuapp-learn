---
title: "Centos Release"
date: 2018-05-29T10:11:05+08:00
draft: false
tags: ["centos","version"]
categories: ["centos"]
subtitle: "CentOS查看系统版本及查看机器位数x86-64"
descriptions: "CentOS 7查看系统版本及查看机器位数x86-64"
bigimg:
---


前言

记下CentOS 7查看系统版本及查看机器位数x86-64的方法，由于不经常使用Linux，每当使用的时候就是安装软件，安装软件的时候就要选择安装包平台，是32位的还是64位的。这时候突然发现不知道怎么查，于是百度。虽然轻而易举百度出来，但仍旧没有自己的笔记看起来舒服。所以，还是记录下来。

## 辨识标准

首先要清楚什么样标识是32位的，什么样的是64位的。

PC server X86 系列

- I386--I686 都是32位
- x86_64 是 64位

## 查看位数命令
命令实在是不要太多，为了防止选择性障碍，一致选择第一种方式，后面的仅作为补充。

### 方法1：

```
[root@linuxidc ~]# uname -a
Linux linuxidc 3.10.0-327.18.2.el7.x86_64 #1 SMP Thu May 12 11:03:55 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```
### 方法2：显示系统程序信息

```
[root@linuxidc ~]# file /bin/ls
/bin/ls: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.32, BuildID[sha1]=aa7ff68f13de25936a098016243ce57c3c982e06, stripped
```
### 方法3：

```
[root@linuxidc ~]# cat /proc/version
Linux version 3.10.0-327.18.2.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.3 20140911 (Red Hat 4.8.3-9) (GCC) ) #1 SMP Thu May 12 11:03:55 UTC 2016
```

### 方法4：

（32位的系统中int类型和long类型一般都是4字节，64位的系统中int类型还是4字节的，但是long已变成了8字节inux系统中可用"getconf WORD_BIT"和
"getconf LONG_BIT"获得word和long的位数。64位系统中应该分别得到32和64。）

```
[root@linuxidc ~]# getconf LONG_BIT
64
```

## 查看系统版本

### 方法1：

```
[root@linuxidc ~]#  lsb_release -a
LSB Version:    :core-4.1-amd64:core-4.1-noarch:cxx-4.1-amd64:cxx-4.1-noarch:desktop-4.1-amd64:desktop-4.1-noarch:languages-4.1-amd64:languages-4.1-noarch:printing-4.1-amd64:printing-4.1-noarch
Distributor ID: CentOS
Description:    CentOS Linux release 7.2.1511 (Core) 
Release:    7.2.1511
Codename:   Core
```

### 方法2：


```
[root@linuxidc ~]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel Fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
RedHat_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

### 方法3：

```
[root@linuxidc ~]# cat /etc/redhat-release
CentOS Linux release 7.2.1511 (Core) 
```

### 方法4：

```
[root@linuxidc ~]# rpm -q centos-release
centos-release-7-2.1511.el7.centos.2.10.x86_64
```
## 查看内核版本

### 方法1：

```
[root@linuxidc ~]# cat /proc/version
Linux version 3.10.0-327.18.2.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.3 20140911 (Red Hat 4.8.3-9) (GCC) ) #1 SMP Thu May 12 11:03:55 UTC 2016
```

### 方法2：

```
[root@linuxidc ~]# uname -a
Linux linuxidc 3.10.0-327.18.2.el7.x86_64 #1 SMP Thu May 12 11:03:55 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```

## Ref

- https://www.linuxidc.com/Linux/2016-11/137550.htm