---
title: "Mysql Use"
date: 2018-03-02T12:41:58+08:00
draft: false
tags: ["mysql"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

mysql系列之mysql使用规范

mysql使用规范

## 级别说明

所有级别：

* 级别1，必须做到
* 级别2，优先做到

级别：1

1. 库名，表名，字段名：使用小写
2. 字符：utf8
3. 表：不使用删除表（`drop`），而使用清空表（`truncate`，或者`delete from`）
4. 表：记录数超过10000的情况下，加索引

级别：2

1. 库名，表名，字段名：使用下划线分割。如：`day_stocka_wind`
2. 表：加主键
3. 清空表：优先使用`truncate`（命令：`truncate table tbl_name;`），其次使用`delete from`(命令：`delete from tbl_name;`）

## 关于删除表

删除表：使用`DROP TABLE tbl_name;`， 或者`DROP TABLE IF EXISTS tbl_name;`

若有业务确实需要删除表的，请联系小鱼。

## end
