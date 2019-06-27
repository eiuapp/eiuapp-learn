---
title: "Mysql Config"
date: 2018-02-12T10:05:24+08:00
draft: false
tags: ["mysql", "config"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---


## [MySQL5.7初始密码查看及重置](http://blog.csdn.net/t1anyuan/article/details/51858911)

```
[root@localhost jlch]# grep 'temporary password' /var/log/mysqld.log
2018-02-12T07:28:21.362804Z 1 [Note] A temporary password is generated for root@localhost: Ngu97xLjPL(S
[root@localhost jlch]#
```

## [MySQL源码安装、配置、初始化及启动](http://www.linuxidc.com/Linux/2016-10/135975.htm)

```
mysql_secure_installation
```

## [配置mysql允许远程连接的方法](http://www.cnblogs.com/linjiqin/p/5270938.html)

```
默认情况下，mysql只允许本地登录，如果要开启远程连接，则需要修改/etc/mysql/my.conf文件。

一、修改/etc/mysql/my.conf
找到bind-address = 127.0.0.1这一行
改为bind-address = 0.0.0.0即可

二、为需要远程登录的用户赋予权限
1、新建用户远程连接mysql数据库
grant all on *.* to admin@'%' identified by '123456' with grant option; 
flush privileges;
允许任何ip地址(%表示允许任何ip地址)的电脑用admin帐户和密码(123456)来访问这个mysql server。
注意admin账户不一定要存在。

2、支持root用户允许远程连接mysql数据库
grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
flush privileges;

```
## [MySQL5.7 添加用户、删除用户与授权](https://www.cnblogs.com/xujishou/p/6306765.html)


```
mysql> CREATE USER 'xieyanke'@'%' IDENTIFIED BY 'xieyanke3306!Q';
Query OK, 0 rows affected (0.00 sec)

mysql> grant all privileges on *.* to 'xieyanke'@'%' identified by 'xieyanke3306!Q' WITH GRANT OPTION;
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

```

## [MYSQL授权当中的with grant option的作用](https://www.cnblogs.com/aguncn/p/4313724.html)

```
mysql> grant all privileges on *.* to test@localhost identified by 'test' with grant option;
```

这句增加一个本地具有所有权限的test用户（超级用户），密码是test。ON子句中的*.*意味着"所有数据库、所有表"。with grant option表示它具有grant权限。 
```
mysql> grant select,insert,update,delete,create,drop privileges on test.* to test1@'192.168.1.0/255.255.255.0' identified by 'test'; 
```
这句是增加了一个test1用户，口令是test，但是它只能从C类子网192.168.1连接，对test库有select,insert,update,delete,create,drop操作权限。 

用grant语句创建权限是不需要再手工刷新授权表的，因为它已经自动刷新了。 

## [mysql create database 指定utf-8编码](http://outofmemory.cn/code-snippet/2533/mysql-create-database-specify-utf-8-coding)

```
mysql> CREATE DATABASE IF NOT EXISTS dps DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
Query OK, 1 row affected (0.00 sec)

mysql>
```

## [mysql5.7设置不区分大小写](http://blog.csdn.net/zhongdajiajiao/article/details/51698022)

```
lower_case_table_names=1
```

验证, 密码是 `***Jlch123!@#***`
```
[root@localhost jlch]# mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.21 MySQL Community Server (GPL)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show variables like '%case_table%';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| lower_case_table_names | 1     |
+------------------------+-------+
1 row in set (0.00 sec)

mysql>
```
