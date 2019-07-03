---
title: "Python sqlalchemy 使用"
date: 2018-02-06T00:08:44+08:00
draft: false
tags: ["sqlalchemy", "python"]
categories: ["python"]
subtitle: ""
descriptions: ""
bigimg:
---

## sqlalchemy 如何在查询的时候排除掉数据库字段为 null的 ##

python语句

```python
UnReadMsg = session.query(MainMongodbfile).filter_by(id=481)
UnReadMsg = session.query(MainMongodbfile).filter_by(indicator_title1 = "年烧煤--吨")
UnReadMsg = session.query(MainMongodbfile).filter_by(indicator_title1 = None)
UnReadMsg = session.query(MainMongodbfile).filter(MainMongodbfile.indicator_title1 != None)

```
能正常使用, 但是 使用下面2行,来查询为 null的, 却会报错

```bash
UnReadMsg = session.query(MainMongodbfile).filter_by(indicator_title1 == None)
UnReadMsg = session.query(MainMongodbfile).filter_by(indicator_title1.isnot(None))
UnReadMsg = session.query(MainMongodbfile).filter(indicator_title1 != None)
UnReadMsg = session.query(MainMongodbfile).filter_by(MainMongodbfile.indicator_title1 != None)
UnReadMsg = session.query(MainMongodbfile).filter_by(indicator_title1 != None)
UnReadMsg = session.query(MainMongodbfile).filter_by( not_(indicator_title1.is_(None)))
UnReadMsg = session.query(MainMongodbfile).filter_by(indicator_title1.isnot(null()))
UnReadMsg = session.query(MainMongodbfile).filter_by(MainMongodbfile.indicator_title1.isnot(null()))
UnReadMsg = session.query(MainMongodbfile).filter_by( not_(indicator_title1.is_(None)))
UnReadMsg = session.query(MainMongodbfile).filter_by( not_(MainMongodbfile.indicator_title1.is_(None)))
UnReadMsg = session.query(MainMongodbfile).filter_by(MainMongodbfile.indicator_title1.isnot(None))
```

报错信息如下:

```bash
Traceback (most recent call last):
  File "mysql_esa_task_indicator.py", line 112, in <module>
    UnReadMsg = session.query(MainMongodbfile).filter_by(indicator_title1.isnot(None))
NameError: name 'indicator_title1' is not defined
(py373) DESKTOP-APB1HCJ%
```

或者

```bash
Traceback (most recent call last):
  File "mysql_esa_task_indicator.py", line 123, in <module>
    UnReadMsg = session.query(MainMongodbfile).filter_by(MainMongodbfile.indicator_title1.isnot(None))
TypeError: filter_by() takes 1 positional argument but 2 were given
```

所以, 

- 选`null`, 则用

```bash
UnReadMsg = session.query(MainMongodbfile).filter_by(indicator_title1 = None)
```

- `not null`的, 则用

```bash
UnReadMsg = session.query(MainMongodbfile).filter(MainMongodbfile.indicator_title1 != None)
```




