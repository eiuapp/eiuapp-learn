---
title: "Mac Sed"
date: 2018-10-08T12:08:31+08:00
draft: false
tags: ["mac", "sed"]
categories: ["mac"]
subtitle: "Mac OS X自带的sed等命令行工具有一些缺陷和不足"
descriptions: ""
bigimg:
---

由于Mac OS X自带的sed等命令行工具是基于BSD的，有一些缺陷和不足，比如sed不支持\t来表示TAB

## Env

- os: mac

报错如下：

```
➜  home git:(master) ✗ grep "finsoft.info" -rl ./CNAME
./CNAME
➜  home git:(master) ✗ 
➜  home git:(master) ✗ grep "finsoft.info" -rl ./CNAME | xargs sed -i "s/finsoft.info/qqbb.app/g"
sed: 1: "./CNAME": invalid command code .
➜  home git:(master) ✗ 
```
## Step

```
➜  ~ brew install coreutils
➜  ~ vi ~/.zshrc
➜  ~ source ~/.zshrc
➜  ~ tail ~/.zshrc
## coreutil
export PATH="$(brew --prefix coreutils)/libexec/gnubin:/usr/local/bin:$PATH"
export MANPATH="$(brew --prefix coreutils)/libexec/gnuman:$MANPATH"
➜  ~ brew install gnu-sed --with-default-names
➜  ~ 
```

## Ref

- https://blog.csdn.net/xicikkk/article/details/52559433

