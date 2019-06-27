---
title: "Mongodb Update"
date: 2018-03-02T11:52:44+08:00
draft: false
tags: ["mongodb", "update"]
categories: ["mongodb"]
subtitle: ""
descriptions: ""
bigimg:
---

mongodb系列之update

只要是批量，后头加 `{multi: true}` 吧。

## 批量，新增一个k-v

db.Signals_Day.update({TrdDt:20161129}, {$set: {"Real": true}}, {multi: true})

    testrs:PRIMARY> db.Signals_Day.update({TrdDt:20161129}, {$set: {"Real": true}}, {multi: true})
    WriteResult({ "nMatched" : 8935, "nUpserted" : 0, "nModified" : 8933 })
    testrs:PRIMARY> db.Signals_Day.find({TrdDt:20161129})
    { "_id" : ObjectId("583d27735868c24cb078e925"), "Type" : "NewHighLow", "Sign" : -1, "ID" : "000785.SZ", "TrdDt" : 20161129, "TrdTm" : 1500, "PrevClsPx" : 14.99, "LastPx" : 14.4, "Real" : true }
    { "_id" : ObjectId("583d27735868c24cb078e92a"), "Type" : "RSI", "Sign" : -1, "ID" : "002117.SZ", "TrdDt" : 20161129, "TrdTm" : 1500, "PrevClsPx" : 29.88, "LastPx" : 29.84, "Real" : true }
    Type "it" for more
    testrs:PRIMARY>_


## 批量，修改一个key的value, 原value乘以100

db.Signals_Day.update({TrdDt:20161129}, {$mul: {"TrdTm":100}}, {multi: true} )

    testrs:PRIMARY> db.Signals_Day.update({TrdDt:20161129}, {$mul: {"TrdTm":100}}, {multi: true} )
    WriteResult({ "nMatched" : 8935, "nUpserted" : 0, "nModified" : 8935 })
    testrs:PRIMARY> db.Signals_Day.find({TrdDt:20161129})

## 手工修改 VIP 用户数

修改某一天

```bash
testrs:PRIMARY> db.user.update({"vip.level":0, "registerTime" : /20180317/}, {$set: {"vip" : { "level" : 1, "value" : "会员", "createTime" : "20180317010101", "endTime" : "20180417235959" }}}, { multi: 1})
WriteResult({ "nMatched" : 8, "nUpserted" : 0, "nModified" : 8 })
```

修改小于某个指定日期

```bash
testrs:PRIMARY> db.user.find({"vip.level":0, "registerTime" : {$lt :"20180317000000"}}).count()
3487
testrs:PRIMARY> db.user.update({"vip.level":0, "registerTime" : {$lt :"20180317000000"}}, {$set: {"vip" : { "level" : 1, "value" : "会员", "createTime" : "20180316010101", "endTime" : "20180516235959" }}}, { multi: 1})
WriteResult({ "nMatched" : 3487, "nUpserted" : 0, "nModified" : 3487 })
testrs:PRIMARY>
```



##
