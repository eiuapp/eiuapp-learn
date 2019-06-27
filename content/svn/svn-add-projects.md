---
title: "Svn Add Projects"
date: 2018-06-28T18:02:21+08:00
draft: false
tags: ["svn"]
categories: ["svn"]
subtitle: "svn增加工程"
descriptions: ""
bigimg:
---


svn增加工程

## Env

- os: ubuntu16
- svn: 1.9.7

## Step

下面以 `ValueAddedChain` 工程为例

### 加工程
```
[root@vachain ~]# svnadmin create /application/svndata/ValueAddedChain
[root@vachain ~]# cd /application/svndata/
[root@vachain svndata]# ls
ValueAddedChain
[root@vachain svndata]# 
```

### 改配置

配置交由全局配置文件


```
[root@vachain svndata]# pwd
/application/svndata/ 
[root@vachain svndata]# vi ValueAddedChain/conf/
```

这里面修改的内容是

```
[general]
anon-access = none
auth-access = write
password-db = /application/svnpasswd/passwd
authz-db = /application/svnpasswd/authz
```

这里的 password-db, authz-db可以是从其它地方copy过来，后，修改

```
cd /application/svndata/sadoc/
cp authz passwd /application/svnpasswd/
cd svnpasswd/
```

当然，你可以直接把已经配置好的文件，copy过来就行了

```
[root@vachain svndata]# cp doc/conf/svnserve.conf ValueAddedChain/conf/
cp: overwrite `ValueAddedChain/conf/svnserve.conf'? y
[root@vachain svndata]# 
```


### 配置完了，要重启

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

