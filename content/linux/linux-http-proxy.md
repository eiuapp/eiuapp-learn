---
title: "Linux Http Proxy"
date: 2018-05-24T11:30:50+08:00
draft: false
tags: ["linux","proxy"]
categories: ["linux"]
subtitle: ""
descriptions: ""
bigimg:
---


linux-proxy 相关

## proxy FQ 

```
root@km:~# cat proxy.sh 
#!/bin/bash
NO_PROXY=localhost,127.0.0.1/8,192.168.31.1/24
export NO_PROXY
export http_proxy=http://192.168.31.10:1080/
export https_proxy=http://192.168.31.10:1080/
root@km:~# 
```

这个问题，可以参考 https://blog.finsoft.info/posts/ubuntu-apt-http-proxy/#%E5%8F%96%E6%B6%88apt%E4%BB%A3%E7%90%86 

##### 终端：
```
[tom@mpro Desktop]$ export http_proxy=http://192.168.31.10:8031  # 这个 http://192.168.31.10:8031  是在 192.168.31.10的机器提供出来的 地址。 192.168.31.10的机器 是代理服务器。
[tom@mpro Desktop]$ export https_proxy=http://192.168.31.10:8031 
[tom@mpro Desktop]$ ping www.google.com.hk # 代理后，ping 依然是不行的。呵呵
PING www.google.com.hk (93.46.8.89) 56(84) bytes of data.
^C
--- www.google.com.hk ping statistics ---
6 packets transmitted, 0 received, 100% packet loss, time 4999ms

[tom@mpro Desktop]$ wget www.google.com # 但是wget 是可以的。
--2016-07-22 11:18:25--  http://www.google.com/
Connecting to 192.168.31.10:8031... connected.
Proxy request sent, awaiting response... 200 OK
Length: unspecified [text/html]
Saving to: ‘index.html’

    [  <=>                                  ] 10,466      11.5KB/s   in 0.9s   

2016-07-22 11:18:26 (11.5 KB/s) - ‘index.html’ saved [10466]

[tom@mpro Desktop]$ vi index.html 
[tom@mpro Desktop]$ 
```
##### 取消 http_proxy


运行： unset http_proxy

```
tom@adata:~$ unset http_proxy
tom@adata:~$ export
declare -x CLUTTER_IM_MODULE="xim"
declare -x COLORTERM="gnome-terminal"
declare -x
...
...
...
```
查看一下 export ，知道，已经没有 http_proxy 了。

##### chrome
先disable(去除)与网络代理相关的插件

设置，network, network proxy, Manual, 

HTTP Proxy: 192.168.31.10 8031

##### firefox
先disable(去除)与网络代理相关的插件

preferences, Advanced, Network, Settings, 

Manual Proxy configuration:

HTTP Proxy: 192.168.31.10 8031 

## ref

- http://bbs.51cto.com/thread-1138093-1.html