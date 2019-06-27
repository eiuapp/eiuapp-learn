---
title: "Network Proxy Win10"
date: 2018-06-11T10:10:16+08:00
draft: false
tags: ["network", "proxy", "windows"]
categories: ["proxy"]
subtitle: "通过win10实现FQ"
descriptions: ""
bigimg:
---

通过win10实现FQ

## Env

- os: win10

## Step

### 先配置Shadowsocks


### privoxy or not

#### 方法1 不配置 privoxy

其实可以不配置privoxy,而使得socks5生效。

具体步骤如下：

![win10-internet-proxy](https://res.cloudinary.com/dmtixvmgt/image/upload/v1528682939/win10-internet-proxy_kjqski.png)

配置的过程中，可以通过一个[pac 文件](https://pan.baidu.com/s/1_JSRnbuoimZ-48kJG4EnfA)过滤url,这个地方，怎么设置的呢？看下图

![win10-internet-proxy-2](https://res.cloudinary.com/dmtixvmgt/image/upload/v1528684087/win10-internet-proxy-2_pspcap.png)


#### 方法2 配置 privoxy

