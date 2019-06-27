+++
title = "在vmware workstation pro 中给ubuntu增加新分区"
date = 2019-01-08T00:00:00+08:00
lastmod = 2019-01-08T19:04:18+08:00
tags = ["ubuntu", "fdisk"]
categories = ["ubuntu"]
draft = false
weight = 3001
+++

## env {#env}

在 vmware workstation pro 中安装的ubuntu server已经把20G的硬盘已经按默认的方式分区，且已经是把20G硬盘全占用。
此时，如果用 fdisk /dev/sda 然后 n 来新建分区的时候，会报出 no sector available 的错误。

这个时候，有2个思路。

1.  把原来的 sector和硬盘空间 释放出来，然后，新建立分区
2.  把硬盘空间加大（不是另增加一块硬盘），然后，重新规划分区


## step {#step}

思路1，本来是寄希望于 gpart 和 gparted 命令（通过 apt install 安装）的，但是，发现，文章已失效，-s -t 等命令的效果与原理明显不对了，所以思路1，中断。

思路2，操作成功。下面就只介绍思路2.

关虚拟机，打开，点击，设置/硬盘/扩展容量，增加10G到30G, 然后，磁盘整理，一下，开机。

这个时候，fdisk -l 可以看到 sector 已经增加了。

但是通过 n 后，并不能选择 l, 只能选择 p，且分配的大小，不超过1个G,明显不是我们想要的。

这个时候，我直接把 原有的 1,2,5 三个分区的 2，5直接删除（d）,然后再 n, 这个时候，就可以分出我们想要的分区了。
最后我的效果是 1,2,5,6 其中6分区占用了新增磁盘的 8 GB .
