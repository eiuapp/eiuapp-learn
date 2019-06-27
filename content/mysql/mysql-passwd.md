---
title: "Mysql Passwd"
date: 2018-03-26T16:18:39+08:00
draft: false
tags: ["mysql", "passwd"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

## mysql插入密码

```
> INSERT INTO `factor_administrator`
(`id_admin`,`name`,`pwd`,`admin`) VALUES (1,'wuxuan',md5('gws20180129'),1);
```
那么, 这时, 用户与密码就是: wuxuan, gws20180129