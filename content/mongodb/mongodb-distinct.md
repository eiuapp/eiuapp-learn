---
title: "Mongodb Distinct"
date: 2018-03-02T11:51:48+08:00
draft: false
tags: ["mongodb", "distinct"]
categories: ["mongodb"]
subtitle: ""
descriptions: ""
bigimg:
---

mongodb系列之distinct 

## reference

https://docs.mongodb.com/manual/reference/method/db.collection.distinct/

http://www.jb51.net/article/65929.htm

## destinct

* MongoDB的destinct命令是获取特定字段中不同值列表。该命令适用于普通字段，数组字段和数组内嵌文档.

mongodb的distinct的语句： db.users.distinct('last_name'）;

等同于 SQL 语句: select DISTINCT last_name from users;

表示的是根据指定的字段返回不同的记录集。

* 如果，有相关的查询，则放在后面。

    testrs:PRIMARY> db.KLine_1Min.distinct("LastPx", {"TrdDt":20161101, "ID": "600650.SH"})
    [
    	25.88,
    	25.89,
    	25.87,
    	25.86
    ]
    testrs:PRIMARY>
## 示例

    > db.KLine_1Min.distinct("ID").length
    114
    > db.KLine_1Min.distinct("ID")

    testrs:PRIMARY> db.KLine_1Min.distinct("ID", {"TrdDt":20161102, "ID": /^60.*SH/, "TrdTm":1441}).length
    1081
    testrs:PRIMARY> db.KLine_1Min.distinct("ID", {"TrdDt":20161101, "ID": /^60.*SH/, "TrdTm":1441}).length
    1109
    testrs:PRIMARY> db.KLine_1Min.distinct("ID", {"TrdDt":20161103, "ID": /^60.*SH/, "TrdTm":1441}).length
    1083
    testrs:PRIMARY> db.KLine_1Min.distinct("ID", {"TrdDt":20161031, "ID": /^60.*SH/, "TrdTm":1441}).length
    1107
    testrs:PRIMARY> db.KLine_1Min.distinct("ID", {"TrdDt":20161025, "ID": /^60.*SH/, "TrdTm":1441}).length
    1077
    testrs:PRIMARY> db.KLine_1Min.distinct("ID", {"TrdDt":20161025, "ID": /^00.*SZ/, "TrdTm":1441}).length
    1168
    testrs:PRIMARY> db.KLine_1Min.distinct("ID", {"TrdDt":20161031, "ID": /^00.*SZ/, "TrdTm":1441}).length
    1271
    testrs:PRIMARY> db.KLine_1Min.distinct("ID", {"TrdDt":20161101, "ID": /^00.*SZ/, "TrdTm":1441}).length
    1271
    testrs:PRIMARY> 
