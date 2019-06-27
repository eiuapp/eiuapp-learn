---
title: "Command Tips"
date: 2018-02-03T14:30:30+08:00
draft: false
tags: ["shell"]
categories: ["shell"]
subtitle: ""
descriptions: ""
bigimg:
---

some useful commands

```
netstat -ef | grep 8080
netstat -r | grep nginx
netstat -a | grep nginx
```

```
vmstat 1
```

```
sudo systemctl daemon-reload
```

```
cat values.yaml | grep -v ^# | grep -v ^$
```

按日期删除文件

```
find /root/gws_sync/tasks -mtime +7 -name "*.csv*" -exec rm -rf {} \; >> ./rm.log 2>&1
```

[删除链接](https://www.cnblogs.com/xiaochaohuashengmi/archive/2011/10/05/2199534.html)

有创建就有删除

```
rm -rf symbolic_name ## 注意不是rm -rf symbolic_name/
```




