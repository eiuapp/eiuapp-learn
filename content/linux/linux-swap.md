---
title: "Linux Swap"
date: 2018-06-22T18:56:28+08:00
draft: false
tags: ["linux"]
categories: ["linux"]
subtitle: "清除刷新swap"
descriptions: ""
bigimg:
---

## Env

- os: linux


## Step

有时运行大量的进程后swap大量占用，达到30%的话机器会变得很慢

可以用以下两个命令清除刷新swap

```
swapoff -a
swapon -a
```

这样swap就还原到初始状态


## Ref

- http://blog.163.com/zhao_jw/blog/static/18058736620121027102932108/