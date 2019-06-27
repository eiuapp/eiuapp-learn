---
title: "Curl"
date: 2018-03-08T17:47:28+08:00
draft: false
tags: ["curl"]
categories: ["curl"]
subtitle: ""
descriptions: ""
bigimg:
---

## 获取header

#### env

要求发送get请求，只看服务器返回的头信息

`curl --head xxxx.com` 这样可以看服务器返回的头信息，但是，发送的是head请求

请问，怎么做？

#### step

```
➜  ~ curl -X GET -I https://www.baidu.com/
HTTP/1.1 200 OK
Server: nginx
Date: Thu, 08 Mar 2018 09:46:48 GMT
Content-Type: text/html;charset=utf-8
Content-Length: 491
Vary: Accept-Encoding
Set-Cookie: vvvv=1; Path=/; Expires=Thu, 08-Mar-18 09:47:48 GMT
Vary: Accept-Encoding
X-Cache: MISS from bs88.10jqka.com.cn
X-Cache: MISS from cachexs
Via: 1.1 bs88.10jqka.com.cn (squid/3.5.20), 1.1 cachexs (squid/3.5.20)
Connection: keep-alive

➜  ~ 
```

## 获取复杂URL内容

```
➜  ~ curl -H "Content-Type:application/json" -X GET -sL https://www.iwencai.com/stockpick/load-data\?typed\=0\&preParams\=\&ts\=1\&f\=1\&qs\=result_original\&selfsectsn\=\&querytype\=stock\&searchfilter\=\&tid\=stockpick\&w\=%E6%98%A8%E6%97%A5%E6%B6%A8%E5%81%9C,%E4%BB%8A%E5%A4%A9%E6%B6%A8%E5%B9%85%3E5%\&queryarea\=
<html><body>
        <script src="//s.thsi.cn/js/chameleon/chameleon.min.1520503.js" type="text/javascript"></script>
        <script language="javascript" type="text/javascript">
        window.location.href="http://www.iwencai.com/stockpick/load-data?typed=0&preParams=&ts=1&f=1&qs=result_original&selfsectsn=&querytype=stock&searchfilter=&tid=stockpick&w=%E6%98%A8%E6%97%A5%E6%B6%A8%E5%81%9C,%E4%BB%8A%E5%A4%A9%E6%B6%A8%E5%B9%85%3E5%&queryarea=";
        </script>
        </body></html>
➜  ~
```

这样, 不是我们想要的结果.
怎么办?



