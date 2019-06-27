---
title: "Linux Port Process"
date: 2018-03-02T11:47:57+08:00
draft: false
tags: ["linux", "port"]
categories: ["linux"]
subtitle: ""
descriptions: ""
bigimg:
---

linux系列之端口与进程

## linux如何查看端口被哪个进程占用

使用netstat 和lsof命令，并用grep来过滤你需要查看的端口。
例如查看tcp有哪些端口打开了：

    netstat -a| grep tcp
然后查看哪个进程占用了这些端口：

    lsof -i
如果要查看某个端口，比如80端口是哪个进程：

    lsof -i | grep :80

如果说，tom用户查到了一些端口号被占用，但是，用lsof却查不到。怎么办？
则要考虑，是不是端口被root用户占用，没有权限。所以要再一次用sudo lsof来查一查。

## 示例

    netstat -ant | grep 8060
    ps -aux | grep node
    lsof -i:8060
    man lsof
    lsof  -i
    lsof  -i -n
    lsof -iTCP
    sudo lsof -i
