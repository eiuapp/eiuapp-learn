---
title: "Xshell Ubuntu Transmitter File"
date: 2018-05-24T11:19:43+08:00
draft: false
tags: ["ubunut", "xshell"]
categories: ["ubuntu"]
subtitle: ""
descriptions: "xshell + ubuntu 直接拖拽完成传输"
bigimg:
---


xshell + ubuntu 直接拖拽完成传输

## env

- win10-xshell
- ubuntu

## step


ubunut 安装lrzsz软件

```
ubuntu@ip-172-31-17-102:~$ sudo apt install lrzsz
Reading package lists... Done
Building dependency tree       
Reading state information... Done
lrzsz is already the newest version (0.12.21-8).
0 upgraded, 0 newly installed, 0 to remove and 1 not upgraded.
ubuntu@ip-172-31-17-102:~$
```

如果第一次没有成功，则，直接再把上面的命令运行一次

传输完成了，会显示如下内容

```
ubuntu@ip-172-31-17-102:~$ rz -E
rz waiting to receive.
ubuntu@ip-172-31-17-102:~$
```