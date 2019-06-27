---
title: "Grep Exclude Many File"
date: 2018-07-03T11:20:35+08:00
draft: false
tags: ["grep"]
categories: ["grep"]
subtitle: "grep 同时排除多个关键字,多个文件"
descriptions: ""
bigimg:
---

grep 同时排除多个关键字,多个文件

## Step

`grep -v 排除文件 | grep -v 排除文件`

```
tom@tom-w10vbud16:~$ ifconfig | grep inet | grep -v inet6 | grep -v 127.0.0.1
          inet addr:172.17.0.1  Bcast:0.0.0.0  Mask:255.255.0.0
          inet addr:10.88.88.130  Bcast:10.88.88.255  Mask:255.255.255.0
          inet addr:10.42.0.0  Bcast:0.0.0.0  Mask:255.255.255.255
tom@tom-w10vbud16:~$ 
```

## Ref

- https://blog.csdn.net/qq70945934/article/details/77573870