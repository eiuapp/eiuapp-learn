---
title: "Mac Rar Unrar"
date: 2018-06-12T17:15:24+08:00
draft: false
tags: ["mac", "rar"]
categories: ["mac"]
subtitle: "Mac上rar文件命令解压和压缩"
descriptions: ""
bigimg:
---

Mac上rar文件命令解压和压缩

## Env

- os: macOS

## Step

rar和unrar命令需要自己安装

可以直接通过brew安装，如果不清楚brew安装命令，可以查看《mac上安装类似 apt-get 的软件包管理器 -- Homebrew》

下面说下另外一种简单安装方式

1.下载mac上对应rar版本

http://www.rarlab.com/download.htm

2.利用tar名解压下载的rarosx-5.4.0.tar.gz，版本可能会更新
```
tar xzvf arosx-5.4.0.tar.gz .  #解压到当前目录
```
3.安装rar和unrar命令
```
sudo install -c -o $USER rar /usr/local/bin/  ＃安装rar
sudo install -c -o $USER unrar /usr/local/bin  ＃安装unrar
```
如果安装失败可以看看`/usr/local/bin` 目录是不是存在rar或unrar的软链接

4.利用rar和unrar压缩和解压文件

rar和unrar文件的参数也很多，就不在一一介绍了，直接在Ternimal执行对应命令就能看到所有参数选项，下面列举几个常用的


解压文件：`unrar x test.rar`
压缩文件A和B：`rar a 压缩后.rar A B`

## Ref

- http://www.rarlab.com/download.htm
- https://blog.csdn.net/yin1031468524/article/details/68955194?locationNum=12&fps=1
