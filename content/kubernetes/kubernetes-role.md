---
title: "Kubernetes Role"
date: 2018-07-01T20:50:36+08:00
draft: false
tags: ["kubernetes"]
categories: ["kubernetes"]
subtitle: "kubernetes的role"
descriptions: ""
bigimg:
---

## Env

- kubernetes: 1.10


## Step

"我打你", 

- 动作执行对象是 `我`,
- 动作是 `打`
- 动作被执行对象是 `你`

在namespace级别：
```
role: 是`动作`与`动作被执行对象`的规则，比如："打你"
rolebing: 是绑定`动作`与`动作对象`，比如：指定 "我"与"打你" 相绑定。
```
在cluster级别：
```
clusterrole
clusterrolebing
```

## Ref

- https://live.vhall.com/829906699