---
title: "Virtualbox Share Pasteboard"
date: 2018-05-24T11:37:55+08:00
draft: false
tags: ["virtualbox"]
categories: ["virtualbox"]
subtitle: ""
descriptions: ""
bigimg:
---

virtubox 共享剪切板

## env

- host: win10 
- vbox-os: ubuntu18-desktop


## step

#### M1, OK

等我们启动虚拟机进入虚拟机后，在虚拟机的运行框的功能栏部分，如图所示点击设备，然后分别可以配置共享粘贴板和拖拽选项，也是可以配置功能的执行方向

#### M2, not OK


我们可以在不启动虚拟机实例的进行设置，先鼠标左键点击选择一个创建好的实例，然后点击功能栏的设置图标，进入到设置的总体页面中,然后我们选择常规选项中的高级一栏，鼠标左键点击进入，我们可以看到有共享粘贴板和拖放两个配置项，其中每一个都可以进行单独设置，如图所示，分别可以设置从虚拟机到物理机，或者相反操作，或者双向操作，这里一般推荐设置双向操作，这样我们的虚拟机和物理机就可以无障碍的进行交互了

## ref 

- https://jingyan.baidu.com/article/4dc40848645918c8d846f14b.html