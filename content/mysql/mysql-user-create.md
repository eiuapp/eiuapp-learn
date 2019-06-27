---
title: "Mysql User Create"
date: 2018-07-04T14:40:22+08:00
draft: false
tags: ["mysql"]
categories: ["mysql"]
subtitle: "mysql创建用户"
descriptions: ""
bigimg:
---

mysql创建用户

## Env

- mysql: 5.7


## Step

```
MariaDB [(none)]> create user 'root'@'%' IDENTIFIED BY 'password';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all on *.* to 'root'@'%' identified by 'password';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush privileges;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]>
```

## Ref

- 