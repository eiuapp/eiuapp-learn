---
title: "Stock Iwencai"
date: 2018-03-08T16:43:27+08:00
draft: false
tags: ["stock", "iwencai"]
categories: ["stock"]
subtitle: ""
descriptions: ""
bigimg:
---

## 搜索股票

#### env

搜索 "昨日涨停,今天涨幅>5%"的股票

https://www.iwencai.com/stockpick/search?querytype=stock&tid=stockpick&w="量价巨增，底部吸筹"

#### step

###### 准备URL

* 方法1

打开[url解码工具](http://www.ofmonkey.com/encode/url)
```
昨日涨停,今天涨幅>5%
```
放到左边, 点击`解码`,得到
```
%e6%98%a8%e6%97%a5%e6%b6%a8%e5%81%9c%2c%e4%bb%8a%e5%a4%a9%e6%b6%a8%e5%b9%85%3e5%25
```

把刚刚得到的结果, 替换到下面`w`后面的位置
```
https://www.iwencai.com/stockpick/load-data?typed=0&preParams=&ts=1&f=1&qs=result_original&selfsectsn=&querytype=stock&searchfilter=&tid=stockpick&w=%E9%87%8F%E4%BB%B7%E5%B7%A8%E5%A2%9E%EF%BC%8C%E5%BA%95%E9%83%A8%E5%90%B8%E7%AD%B9&queryarea=
```
成
```
https://www.iwencai.com/stockpick/load-data?typed=0&preParams=&ts=1&f=1&qs=result_original&selfsectsn=&querytype=stock&searchfilter=&tid=stockpick&w=%e6%98%a8%e6%97%a5%e6%b6%a8%e5%81%9c%2c%20%e4%bb%8a%e5%a4%a9%e6%b6%a8%e5%b9%85%3e5%25&queryarea=
```

* 方法2

把搜索条件 替换到下面`w`后面的位置
```
https://www.iwencai.com/stockpick/load-data?typed=0&preParams=&ts=1&f=1&qs=result_original&selfsectsn=&querytype=stock&searchfilter=&tid=stockpick&w=%E9%87%8F%E4%BB%B7%E5%B7%A8%E5%A2%9E%EF%BC%8C%E5%BA%95%E9%83%A8%E5%90%B8%E7%AD%B9&queryarea=
```
成
```
https://www.iwencai.com/stockpick/load-data?typed=0&preParams=&ts=1&f=1&qs=result_original&selfsectsn=&querytype=stock&searchfilter=&tid=stockpick&w=昨日涨停,今天涨幅>5%&queryarea=
```
###### 得到股票

* 浏览器

然后, 打开刚刚得到的 url, 就能得到我们想要的股票了

* curl

没成功

```
➜  src git:(master) ✗ curl -sL https://www.iwencai.com/stockpick/load-data\?typed\=0\&preParams\=\&ts\=1\&f\=1\&qs\=result_original\&selfsectsn\=\&querytype\=stock\&searchfilter\=\&tid\=stockpick\&w\=昨日涨停,今天涨幅\>5%\&queryarea\=
<html><body>
        <script src="//s.thsi.cn/js/chameleon/chameleon.min.1520561.js" type="text/javascript"></script>
        <script language="javascript" type="text/javascript">
        window.location.href="http://www.iwencai.com/stockpick/load-data?typed=0&preParams=&ts=1&f=1&qs=result_original&selfsectsn=&querytype=stock&searchfilter=&tid=stockpick&w=昨日涨停,今天涨幅>5%&queryarea=";
        </script>
        </body></html>
➜  src git:(master) ✗
```

* crawler

这个部分见, stock-iwencai-crawler.md 文件

