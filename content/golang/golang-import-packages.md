---
title: "Golang Import Packages"
date: 2018-02-09T22:29:31+08:00
draft: false
tags: ["golang"]
categories: ["golang"]
subtitle: ""
descriptions: ""
bigimg:
---

## ref

https://segmentfault.com/q/1010000010846304

https://my.oschina.net/zlLeaf/blog/174404

## step

```
通常情况下，import的包都是相对$GOPATH/src目录引入的，比如从github上面clone下来的项目，直接放到$GOPATH/src目录下，就可以直接import。例如：
如果项目的import路径是这样写的：
import "github.com/yourname/projectname"
需要将项目代码放置在：
$GOAPTH/src/github.com/yourname/projectname/下

如果项目的import是这样写的：
import "message"
则将message.go放到:
$GOAPTH/src/message/目录下即可。
```

