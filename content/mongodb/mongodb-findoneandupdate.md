---
title: "Mongodb Findoneandupdate"
date: 2018-03-02T11:50:44+08:00
draft: false
tags: ["mongodb"]
categories: ["mongodb"]
subtitle: ""
descriptions: ""
bigimg:
---

mongodb系列之findOneAndUpdate 

## 参考

http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html#findOneAndUpdate

https://docs.mongodb.com/manual/reference/method/db.collection.findOneAndUpdate/

## 示例

修改键（字段）的值,修改了{b:2}

    var MongoClient = require('mongodb').MongoClient;
    MongoClient.connect('mongodb://192.168.31.240:27017/test', function(err, db) {
      // Get the collection
      var col = db.collection('find_one_and_update');
      col.insertMany([{a:1, b:1}], {w:1}, function(err, r) {
        col.findOneAndUpdate({a:1}
          , {$set: {b:2}}
          , {  upsert: true
            }
          , function(err, r) {
            db.close();
        });
      });
    });

增加新的键，增加了{d:1}

    var MongoClient = require('mongodb').MongoClient,
      test = require('assert');
    MongoClient.connect('mongodb://192.168.31.240:27017/test', function(err, db) {
      // Get the collection
      var col = db.collection('find_one_and_update');
      col.insertMany([{a:1, b:1}], {w:1}, function(err, r) {

        col.findOneAndUpdate({a:1}
          , {$set: {d:1}}
          , {  upsert: true
            }
          , function(err, r) {

            db.close();
        });
      });
    });
