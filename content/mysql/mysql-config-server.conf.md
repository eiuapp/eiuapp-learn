---
title: "Mysql Config Server.conf"
date: 2018-02-26T17:25:30+08:00
draft: false
tags: ["mysql", "config"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

#### 压缩binlog空间

在运行前, 先去数据目录文件夹下查看一下, `mysql.000001`的文件有多少,并看一下各文件的创建时间.也就是确认一下要删除的binlog日期.

- 按文件序列号删除

如果想要删除`mysql.000050`(不包含)日期之前的binlog文件,
在mysql下运行这个`purge binary logs to 'mysql.000050';`

运行这个后, 会将 数据目录文件夹下的 `mysql.000001`-`mysql.000049`的文件删除(如果想删除更多, 修改数字 `mysql.000050` 为 `mysql.000120` 等).

- 按时间删除

删除2018-02-15 23:59:59 之前binlog。
`purge binary logs before '2018-02-15 23:59:59';`

- 通过配置文件删除8天之前的binlog

同时,在配置文件里添加这个：`expire_logs_days=8`,这个参数表示删除8天之前的binlog
```
expire_logs_days=8
```

