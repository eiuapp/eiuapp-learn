---
title: "Ubuntu Install Faq A"
date: 2018-06-26T18:51:29+08:00
draft: false
tags: ["ubuntu", "install"]
categories: ["ubuntu"]
subtitle: "U盘安装Ubuntu16.04 server版 提示无法挂载cd-rom数据的解决办法"
descriptions: ""
bigimg:
---

U盘安装Ubuntu16.04 server版 提示无法挂载cd-rom数据的解决办法

## Step

错误：

使用类似ultraiso的刻录软件会出现这个错误
`Failed to copy files from CD-ROM, retry?`

解决：

解决的办法是使用[win32diskimager](https://sourceforge.net/projects/win32diskimager/?source=typ_redirect)制作U盘安装程序，就可以正常安装Ubuntu 16.04 Server。

下载地址[win32diskimager](https://sourceforge.net/projects/win32diskimager/?source=typ_redirect)

文本框用来输入文件完整地址，后面的文件夹图标是浏览窗口，默认只能识别img文件。
只需要将iso文件全路径输入在Image File中。
填好镜像的完整地址后右边有个下拉列表用来选择移动设备，千万别选错了！

建议只插一个U盘，以免误操作。之后点击Wirte按钮就开始写入。写入后就能够使用U盘安装了。



## Ref

- http://blog.51cto.com/gentle/1743114
- https://blog.csdn.net/w_ww_w/article/details/18219911
- https://sourceforge.net/projects/win32diskimager/?source=typ_redirect

