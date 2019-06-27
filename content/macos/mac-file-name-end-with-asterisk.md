+++
title = "Mac下为什么有的文件名后带一个*星号"
date = 2018-11-20T00:00:00+08:00
lastmod = 2018-11-20T00:33:09+08:00
tags = ["mac", "file"]
categories = ["mac"]
draft = false
weight = 3002
+++

## Mac下为什么有的文件名后带一个\*星号？ {#mac下为什么有的文件名后带一个-星号}

这个\*号仅仅是ls命令显示的，表示有可执行权限，实际文件名不带\*号。

```
$ ls -F
```

可执行文件名后就会加\*号。

显示一个或多个文件的相关信息。
ls [options] [file-list]


## 参数 {#参数}

默认情况下，ls按照文件名的字母顺序列出文件的信息，file-list可以是任意文件或目录
当file-list包含多个目录时，ls将显示目录的名称，不显示子目录和子文件
当file-list为普通文件时，ls则显示该文件的相关信息


## Ref {#ref}

-   <http://www.cnblogs.com/jackbo/p/7201885.html>
