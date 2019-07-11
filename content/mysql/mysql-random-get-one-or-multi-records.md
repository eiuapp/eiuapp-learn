---
title: "Mysql随机获取一条或者多条数据"
date: 2018-03-02T12:51:05+08:00
draft: false
tags: ["mysql"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

https://www.cnblogs.com/leezhxing/p/3951801.html

```mysql
select id, name, image from device_rent_manage where functionType = 'JCSB' and type = 'YNSBXLJC' and  id >= ((SELECT MAX(id) FROM device_rent_manage where functionType = 'JCSB' and type = 'YNSBXLJC')-(SELECT MIN(id) FROM device_rent_manage where functionType = 'JCSB' and type = 'YNSBXLJC')) * RAND() + (SELECT MIN(id) FROM device_rent_manage where functionType = 'JCSB' and type = 'YNSBXLJC' )  LIMIT 3;
```

