---
title: "Linux Install Ab Httpd Tools"
date: 2018-03-01T17:30:25+08:00
draft: false
tags: ["linux", "ab", "httpd-tools"]
categories: ["linux"]
subtitle: ""
descriptions: ""
bigimg:
---

## env

长城证券 压力测试 要安装 ab

## step

#### 离线安装

下载

- [httpd-tools](http://rpmfind.net/linux/RPM/centos/7.4.1708/x86_64/Packages/httpd-tools-2.4.6-67.el7.centos.x86_64.html)
- [libapr-1.so.0](http://rpmfind.net/linux/rpm2html/search.php?query=libapr-1.so.0()(64bit))
- [libaprutil-1.so.0](http://rpmfind.net/linux/rpm2html/search.php?query=libaprutil-1.so.0()(64bit))

安装

```
rpm -ivh *.rpm
which ab
```

