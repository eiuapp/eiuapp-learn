---
title: "Qor Character Set Database Utf8"
date: 2018-07-04T10:53:56+08:00
draft: false
tags: ["qor", "mysql"]
categories: ["qor"]
subtitle: "qor通过字符集解决输入中文报错"
descriptions: ""
bigimg:
---

qor通过字符集解决输入中文报错

## Env

- os: ubuntu16
- go: 1.10.2

## Step

输入中文会有 `Error 1366: Incorrect string value: '\xE4\xB8\xAD' for column 'name' at row 1`这样的报错。

这个错误主要的原因，就是插入数据的时候，数据库不认这个数据。那我们第一个想到的就是字符集的问题了。（可以先手动 insert into 方式测试）

### mysql 查看字符集

新安装的mysql数据库, 默认字符集是 `latin1`.


```
mysql> show collation like 'gbk%';
+----------------+---------+----+---------+----------+---------+
| Collation      | Charset | Id | Default | Compiled | Sortlen |
+----------------+---------+----+---------+----------+---------+
| gbk_chinese_ci | gbk     | 28 | Yes     | Yes      |       1 |
| gbk_bin        | gbk     | 87 |         | Yes      |       1 |
+----------------+---------+----+---------+----------+---------+
2 rows in set (0.00 sec)

mysql> show variables like 'character_set_server';
+----------------------+--------+
| Variable_name        | Value  |
+----------------------+--------+
| character_set_server | latin1 |
+----------------------+--------+
1 row in set (0.01 sec)

mysql> show variables like 'character_set_database';
+------------------------+--------+
| Variable_name          | Value  |
+------------------------+--------+
| character_set_database | latin1 |
+------------------------+--------+
1 row in set (0.00 sec)

mysql> show variables like 'collation_database';
+--------------------+-------------------+
| Variable_name      | Value             |
+--------------------+-------------------+
| collation_database | latin1_swedish_ci |
+--------------------+-------------------+
1 row in set (0.00 sec)

mysql> 
```

### mysql create database 指定utf-8编码

如下脚本创建数据库yourdbname，并制定默认的字符集是utf8

```
CREATE DATABASE IF NOT EXISTS yourdbname DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

如果要创建默认gbk字符集的数据库可以用下面的sql:

```
create database yourdb DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;
```

先 drop database, 再 create 


```
mysql> drop database qor_test;
Query OK, 9 rows affected (0.26 sec)

mysql> CREATE DATABASE IF NOT EXISTS qor_test DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
Query OK, 1 row affected (0.00 sec)

mysql> 
```

### 再查看字符集

```
mysql> show variables like 'character_set_database';
+------------------------+--------+
| Variable_name          | Value  |
+------------------------+--------+
| character_set_database | latin1 |
+------------------------+--------+
1 row in set (0.00 sec)

mysql> use qor_test;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show variables like 'character_set_database';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| character_set_database | utf8  |
+------------------------+-------+
1 row in set (0.00 sec)

mysql> 
```

### 确认

再启动项目并测试，成功了！


## Ref

- https://jingyan.baidu.com/article/fea4511a011e6bf7bb912518.html
- https://blog.csdn.net/qq_20480611/article/details/48132343

