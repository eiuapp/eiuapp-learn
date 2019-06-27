---
title: "Gitlab Authority"
date: 2018-07-12T11:07:08+08:00
draft: false
tags: ["gitlab"]
categories: ["gitlab"]
subtitle: "gitlab访问,角色与权限"
descriptions: ""
bigimg:
---

## Env

- gitlab: 10.0.8


## Step

公司切入Gitlab来管理代码已经有一年多了，其中遇到很多权限问题，如没有权限clone、没有权限提交代码等等，这里做个总结. 权限分为访问权限和行为权限两个层次.

### 访问权限 – Visibility Level

这个是在建立项目时就需要选定的，主要用于决定哪些人可以访问此项目，包含3种

- Private – 私有，只有属于该项目成员才有原先查看
- Internal – 内部，用个Gitlab账号的人都可以clone
- Public – 公开，任何人可以clone

行为权限
在满足行为权限之前，必须具备访问权限（如果没有访问权限，那就无所谓行为权限了），行为权限是指对该项目进行某些操作，比如提交、创建问题、创建新分支、删除分支、创建标签、删除标签等.

### 角色
Gitlab定义了以下几个角色:

- Guest – 访客
- Reporter – 报告者; 可以理解为测试员、产品经理等，一般负责提交issue等
- Developer – 开发者; 负责开发
- Master – 主人; 一般是组长，负责对Master分支进行维护
- Owner – 拥有者; 一般是项目经理

### 权限
不同角色，拥有不同权限，下面列出Gitlab各角色权限

## Ref

- https://www.cnblogs.com/dzcWeb/p/8919970.html
- http://ju.outofmemory.cn/entry/273281