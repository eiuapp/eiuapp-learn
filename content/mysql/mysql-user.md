---
title: "Mysql User"
date: 2018-03-02T12:47:29+08:00
draft: false
tags: ["mysql", "user"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

mysql系列之MySQL查看用户权限


## reference

http://www.oschina.net/code/snippet_222150_12541

##

show grants for 你的用户;
show grants for root@'localhost';
show grants for webgametest@10.3.18.158;
show create database dbname;  这个可以看到创建数据库时用到的一些参数。
show create table tickets;    可以看到创建表时用到的一些参数
