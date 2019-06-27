+++
title = "mysql 常用用法"
date = 2019-02-13T00:00:00-08:00
lastmod = 2019-02-13T18:09:25-08:00
tags = ["mysql"]
categories = ["mysql"]
draft = false
+++

mysql 常用用法

### 修改mysql用户密码

```mysql
select User,Password from mysql.user;
set password for keystone@localhost = password('324dcdc2318be07d0300');
```

### mysql timestamp

在建立表时,优先使用`ON UPDATE CURRENT_TIMESTAMP`, 也就是说, 优先使用下面的 

```mysql
  `create_date` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_date` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
```

而不是使用这下面的

```bash
`create_date` timestamp NULL DEFAULT NULL COMMENT '创建时间',
`update_date` timestamp NULL DEFAULT NULL COMMENT '更新时间',
```

如果表已经建立好了,则通过 navicat ,选中字段后,点击,`默认:根据当前时间戳更新`, 就可以更改了.



