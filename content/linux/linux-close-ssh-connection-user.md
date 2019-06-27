+++
title = "linux 关闭SSH 连接用户"
date = 2019-01-22T00:00:00-08:00
lastmod = 2019-01-22T00:45:09-08:00
tags = ["linux", "ssh"]
categories = ["linux"]
draft = false
weight = 3001
+++

1.查明登陆端口；

```shell
$ who
root pts/1 Apr 8 00:06 (172.29.0.29)
root pts/2 Apr 8 04:15 (172.29.0.21)
```

2.通知该用户将要关闭他：

```shell
$ echo "I will close your connection" > /dev/pts/2
```

这样他的终端将显示该信息。

3.关闭用户连接

```shell
$ fuser -k /dev/pts/2
```

这个在某些命令，导致ssh后续无法操作时，我们就可以使用这个方式。

![figure src="](https://res.cloudinary.com/dmtixvmgt/image/upload/v1548146143/linux-who-pts-fuser%5Fw5fvlm.png)

疑问：当有多个ssh 连接时，那怎么知道我们要kill 的是哪个 pts 呢？
