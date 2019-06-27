---
title: "Svn Permission Configuration"
date: 2018-06-28T17:42:24+08:00
draft: false
tags: ["svn"]
categories: ["svn"]
subtitle: "svn 修改权限"
descriptions: ""
bigimg:
---

svn 修改权限

## Env

- os: ubuntu16
- svn: 1.9.7
- 目录： /application/

## Step


修改权限文件 `authz`

```
[root@vachain application]# pwd
/application
[root@vachain application]# vi svnpasswd/authz 
```

配置组及组员
```
[groups]
# harry_and_sally = harry,sally
# harry_sally_and_joe = harry,sally,&joe
# [/foo/bar]
# harry = rw
# &joe = r
# * =
vachain=zhaoguofen,yangqing,wutao,zengxianwen,gaojunying,liujiansen,liruizhang,huangyanya,aichao
admin1=zengyunlong,liuchao,hezhengwen
chainexchange=gaojunying
chaintrack=zhaoguofen
chainvac=yangqing,liujiansen
chainpaper=zengxianwen
```

配置项目的权限(以`ripple-1.0`为例)
```
[ripple-1.0:/]
@admin1=rw
@vachain=rw
liuchao=rw
zhouxiaoming=rw
liujiansen=rw
```

配置完了，要重启哟。

```
[root@vachain svndata]# netstat -tlpn | grep svn
tcp        0      0 0.0.0.0:3690                0.0.0.0:*                   LISTEN      22584/svnserve      
[root@vachain svndata]# kill 22584
[root@vachain svndata]# svnserve -d -r /application/svndata/
[root@vachain svndata]# netstat -tlpn | grep svn
tcp        0      0 0.0.0.0:3690                0.0.0.0:*                   LISTEN      22703/svnserve      
[root@vachain svndata]#
```

## Ref

