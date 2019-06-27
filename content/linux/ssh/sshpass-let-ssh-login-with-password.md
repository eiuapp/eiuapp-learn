+++
title = "ssh登录时如何直接在参数中加入登录密码"
date = 2019-01-28T00:00:00-08:00
lastmod = 2019-01-28T22:32:42-08:00
tags = ["linux", "ssh"]
categories = ["linux"]
draft = false
weight = 3004
+++

sshpass: 用于非交互的ssh 密码验证

　　ssh登陆不能在命令行中指定密码，也不能以shell中随处可见的，sshpass 的出现，解决了这一问题。它允许你用 -p 参数指定明文密码，然后直接登录远程服务器。 它支持密码从命令行,文件,环境变量中读取。

　　办法找到了，现在先在自己机器上安装。

　　对于debian/ubuntu系统来说，安装方式很简单：

```shell
sudo apt-get install sshpass
```

　　对于其他系统来说，可以通过编译源码：

```shell
wget http://sourceforge.net/projects/sshpass/files/sshpass/1.05/sshpass-1.05.tar.gz
tar xvzf sshpass-1.05.tar.gz
./configure
make
sudo make install
```

　　./configure 后可以添加参数指定安装目录，比如：

```shell
./configure --prefix=/usr/local/Cellar/sshpass/1.05
```

　　来把sshpass安装到自己喜欢的位置，如果没有这个参数，则安装到默认位置。

　　安装好了后，输入sshpass来查看是否安装好了：

```shell
➜  ~ git:(master) ✗ sshpass -V
sshpass 1.05 (C) 2006-2011 Lingnu Open Source Consulting Ltd.
This program is free software, and can be distributed under the terms of the GPL
See the COPYING file for more information.
➜  ~ git:(master) ✗
```

　　这样就可以通过：

```shell
sshpass -p [passwd] ssh -p [port] root@192.168.X.X
```

　　来登录远程主机了。


## Ref {#ref}

-   <https://www.cnblogs.com/linxiong945/p/4226211.html>
