---
title: "Linux Wgetrc Wget Proxy"
date: 2018-05-24T11:24:18+08:00
draft: false
tags: ["linux","wget", "proxy"]
categories: ["proxy"]
subtitle: ""
descriptions: "wgetrc打开 wget 的代理"
bigimg:
---

wgetrc打开 wget 的代理

## env
- centos

## step
通过 /etc/wgetrc 打开 wget 的代理

```
[root@www ~]# vim /etc/wgetrc
#http_proxy = http://proxy.yoyodyne.com:18023/  <==找到底下这几行，大约在 78 行
#ftp_proxy = http://proxy.yoyodyne.com:18023/
#use_proxy = on

# 将他改成类似底下的模样，记得，你必须要有可接受的 proxy 主机才行！
http_proxy = http://192.168.31.10:8031/
use_proxy = on
# use_proxy=off ## 关闭
[root@www ~]# 
```

## ref
- http://cn.linux.vbird.org/linux_server/0140networkcommand.php#wget
