---
title: "virtualbox加载新的image"
tags: ["virtualbox"]
categories: ["virtualbox"]
date: "2018-01-31T12:26:38+08:00"
subtitle: "加载新image"
draft: false
# descriptions: "Kubernetes作为云原生应用的最佳部署平台，已经开放了容器运行时接口（CRI）、容器网络接口（CNI）和容器存储接口（CSI），这些接口让Kubernetes的开放性变得最大化，而Kubernetes本身则专注于容器调度。本文节选自Kubernetes Handbook - jimmysong.io"
# bigimg: [{src: "https://res.cloudinary.com/jimmysong/image/upload/images/2018012901.jpg", desc: "Fantasy"}]
---

把 从其它地方复制过来的virtualbox 文件, 转移到此机器.

## env

把 harbor 转移

## step

virtualbox/file/add, choose *.vbox 

发现当文件很大时, virtualbox所有的资源都给它了, 基本上就不能动了.


从这里看出来, 在生产环境下, 不能使用 virtualbox 这样的形式, 一旦有了类似这样的占用cpu的操作, 其它机器都卡死了.
