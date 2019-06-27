---
title: "Git Command"
date: 2018-03-02T11:41:30+08:00
draft: false
tags: ["git", "command"]
categories: ["git"]
subtitle: ""
descriptions: ""
bigimg:
---



## git reset --hard commit_id

Git允许我们在版本的历史之间穿梭，使用命令
```
git reset --hard commit_id
```

## 去除对　已添加的文件或文件夹　的跟踪

```
git rm -r --cached -- .idea/
```
这样呢，文件夹里 .idea/　的修改，git都不跟踪了。
```
git rm -r --cached -- node_modules/
```

## git clone 指定分支

```
git clone -b test-v1 git@xxx:xxx.git 
```

## 使用git stash命令保存和恢复进度

[使用git stash命令保存和恢复进度](http://blog.csdn.net/daguanjia11/article/details/73810577)

## 最近一次提交的 hash 短字符串, 如"de34928"

```
git rev-parse --short HEAD
```

## ref

- [git clone 指定分支操作](http://blog.csdn.net/yun__yang/article/details/74466059)
