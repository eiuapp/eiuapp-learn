---
title: "Update Kernel Base Ubuntu"
date: 2018-06-21T21:55:37+08:00
draft: false
tags: ["ubuntu", "kernel"]
categories: ["ubuntu"]
subtitle: "ubuntu16.04更新内核"
descriptions: ""
bigimg:
---


ubuntu16.04 update-kernel

## Env

- os: ubuntu16.04
- ip: 192.168.31.118


## Step

浏览器打开 <http://kernel.ubuntu.com/~kernel-ppa/mainline/>

找到适合的 内核版本（这时v4.12),
进入，

找到合适的内核文件(linux-image-4.12.0-041200-generic\_4.12.0-041200.201707022031\_amd64.deb)

    wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.12/linux-image-4.12.0-041200-generic_4.12.0-041200.201707022031_amd64.deb

然后安装就可以了。

## Ref

- http://kernel.ubuntu.com/~kernel-ppa/mainline/