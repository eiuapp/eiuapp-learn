---
title: "Linux Command"
date: 2018-03-26T16:21:02+08:00
draft: false
tags: ["linux", "command"]
categories: ["linux"]
subtitle: ""
descriptions: ""
bigimg:
---

## 查看 机器的连接数

```
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
```
