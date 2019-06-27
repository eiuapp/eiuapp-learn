+++
title = "linux 软连接 不能加/号"
date = 2019-01-24T00:00:00-08:00
lastmod = 2019-01-24T20:14:48-08:00
tags = ["linux"]
categories = ["linux"]
draft = false
weight = 3002
+++

对某个目录创建符号连接

[root@A home]# ln -s /home/kk /home/abc

此命令表示在/home目录下创建一个链接到/home/kk目录的名字为abc的符号连接。

注意：

-   目标是一个软连接符号，所以不要带上 / 号。带上后，就是指把 软连接放在这个目录下

```shell
➜  org git:(master) ✗ ln -s /home/ubuntu/tom/jinweilai/blog-jwl/org-jwl/org/ ~/org
➜  org git:(master) ✗ ln -s /home/ubuntu/tom/jinweilai/blog-jwl/org-jwl/org/ ~/org/
➜  org git:(master) ✗ ls ~/org
gtd.org  org
➜  org git:(master) ✗
```


## Ref {#ref}

-   <https://blog.csdn.net/liuzhenwen/article/details/8152764>
