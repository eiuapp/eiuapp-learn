---
title: "Bitbucket User Share Repo"
date: 2018-07-07T16:29:25+08:00
draft: false
tags: ["bitbucket"]
categories: ["bitbucket"]
subtitle: "bitbucket组来实现不同用户间的共同管理某一个具体repo"
descriptions: ""
bigimg:
---

bitbucket组来实现不同用户间的共同管理某一个具体repo

## Env

2个bitbucket

- R: repo, 仓库R
- G: 组
- A: UserA, 拥有仓库R，拥有组G，要分享给用户B
- B：UserB, 接受分享，来管理R

## Step

登录A用户，新建立一个分组G

管理组G, 添加用户B

登录bitbucket>进入仓库R>设置>用户和组的访问>用户组>选择一个组G>管理>添加

退出登录

登录B用户, 确认加入组G

git clone git@bitbucket.org:UserA/Repo.git

这样就实现了不同用户间的共同管理某一个具体repo, 
而且，如果不希望用户B继续管理repo,则只需要从 组G中把 用户B 删除就可以了。

## TODO

后续，真的是可以直接将gitee中的私有项目放到 bitbucket中

gitee 只有一个优点，拉取速度还行，但是，这个好像没有什么用。
- 开放项目，没必要放上面，比不过github
- 私有项目，限制多，而且功能没有bitbucket强


## Ref

- 