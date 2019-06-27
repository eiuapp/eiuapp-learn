+++
title = "初始化安装 taro-msparis"
date = 2019-02-07T00:00:00+08:00
lastmod = 2019-02-07T23:47:10+08:00
tags = ["react", "taro"]
categories = ["react"]
draft = false
weight = 3001
+++

## appjson ["tabBar"]["borderStyle"] be "white" or "black" {#appjson-tabbar-borderstyle-be-white-or-black}

dist/app.json中的 "tabBar"下的"borderStyle"要修改为： "black"


## 微信小程序报错request:fail url not in domain list {#微信小程序报错request-fail-url-not-in-domain-list}

报错提示说请求的url不在域名列表里，应该是还没有配置服务器域名，可点击开发者工具右上角 详情-域名信息，看看是否配置了域名；
不过没有配置域名其实开发者工具也是不能发送请求的；
文档：<https://developers.weixin.qq.com/miniprogram/dev/api/wx.request.html>

然后，操作如下：

-   开发者工具里面要勾选不校验，也就是下面这句前打上勾：

Does not verify valid domain names, web-view (business domain names), TLS versions and HTTPS certificates

-   手机里面也要打开调试，也就是：

开发版小程序不是右上角有三个点点嘛，点击就能看到打开调试

这样就可以.
