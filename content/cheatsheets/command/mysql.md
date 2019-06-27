---
title: "Mysql Command"
date: 2018-03-26T16:23:40+08:00
draft: false
tags: ["mysql", "command"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

## 查看 MYSQL连接数

```
show processlist;
```

## 设置mysql远程连接root权限

```
MariaDB [(none)]> Grant all privileges on *.* to 'root'@'%' identified by 'mysql' with grant option;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> 
```
