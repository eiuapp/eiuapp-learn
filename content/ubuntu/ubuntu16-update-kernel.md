+++
title = "ubuntu16.04升级内核至 4.10 以上"
date = 2018-11-23T00:00:00+08:00
lastmod = 2018-11-23T16:16:09+08:00
tags = ["ubuntu", "update-kernel"]
categories = ["ubuntu"]
draft = false
weight = 3001
+++

## ubuntu16.04 update-kernel {#ubuntu16.04-update-kernel}


### env {#env}

```
192.168.31.118
192.168.31.119
```


### step {#step}

浏览器打开 <http://kernel.ubuntu.com/~kernel-ppa/mainline/>

找到适合的 内核版本（这时v4.12), 进入，

找到合适的内核文件(linux-image-4.12.0-041200-generic\_4.12.0-041200.201707022031\_amd64.deb)

```
wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.12/linux-image-4.12.0-041200-generic_4.12.0-041200.201707022031_amd64.deb
```

然后安装就可以了。
