+++
title = "mkfs.xfs command not found"
date = 2019-01-20T00:00:00-08:00
lastmod = 2019-01-21T18:09:25-08:00
tags = ["linux", "mkfs"]
categories = ["linux"]
draft = false
+++

如果 `mkfs.xfs /dev/sdb1` 出现 `mkfs.xfs command not found` , 则运行下面命令安装 mkfs.xfs

```shell
apt-get install xfsprogs
```

## ref
- http://blog.51cto.com/yangzhiming/2117942