---
title: "Mongodb Output Import"
date: 2018-02-06T11:12:03+08:00
draft: false
tags: ["mongodb", "export", "import"]
categories: ["mongodb"]
subtitle: ""
descriptions: ""
bigimg:
---

## env

从 192.168.31.42/abc/Signals_1Min 转移mongodb数据 到 192.168.31.240/openQuote/Signals_1Min

## ref

[MongoDB导入导出以及数据库备份](https://www.cnblogs.com/qingtianyu2015/p/5968400.html)

## step

#### mongoexport 导出

准备导出字段

```
[jlch@check 42]$ cat field_Signals_1Min.txt
_id
Type
Sign
ID
TrdDt
TrdTm
PrevClsPx
LastPx
TM
Vol[jlch@check 42]$
```

导出

```
[jlch@check 43]$ mongoexport -h 192.168.31.42 -d abc -c Signals_1Min -q '{"TrdDt":20180131, "TrdTm":{$lt:141900}, "ID":/SZ/ }' --fieldFile=./field_Signals_1Min.txt --type=csv -o ./Signals_1Min.csv
2018-02-06T11:55:38.885+0800	connected to: 192.168.31.42
2018-02-06T11:55:39.053+0800	exported 3256 records
[jlch@check 42]$ ls
field_Signals_1Min.txt  Signals_1Min.csv
[jlch@check 42]$
```

此时生成 Signals_1Min.csv

#### mongoimport 导入

```
[jlch@check 42]$ mongoimport  -h 192.168.31.240 -d openQuote -c Signals_1Min --file ./Signals_1Min.csv --type csv --fieldFile=./field_Signals_1Min.txt
2018-02-06T11:42:43.521+0800	connected to: 192.168.31.240
2018-02-06T11:42:43.890+0800	imported 4116 documents
[jlch@check 42]$
```

#### draft

```
pwdd=${PWD}
table=KLine_1Min
table=KLine_15Min
table=KLine_5Min
mongoexport -h 192.168.31.42 -d abc -c $table -q '{"TrdDt":20180131, "TrdTm":{$lt:1420}, "ID":/SZ/ }' --fieldFile=$pwdd/field_$table.txt --type=csv -o $pwdd/$table.csv
mongoimport -h 192.168.31.240 -d openQuote -c $table --file ./$table.csv --type csv  --fieldFile=$pwdd/field_$table.txt 
```