---
title: "Markdown Comments"
date: 2018-06-16T23:01:25+08:00
draft: false
tags: ["markdown"]
categories: ["markdown"]
subtitle: "在Markdown中写注释"
descriptions: ""
bigimg:
---

概述
下面是我整理的在Markdown中写注释的几种方法，供自己开发时参考，相信对其他人也有用。

## Step
### html标签
既然Markdown内嵌html语法，那么就可以用可以用隐藏的html标签。

注意：需要在前面空一行

```html

<div style='display: none'>
哈哈我是注释，不会在浏览器中显示。
我也是注释。
</div>
```

### html注释
既然支持html语法，那也支持html注释。

```html

<!--哈哈我是注释，不会在浏览器中显示。-->

<!--
哈哈我是多段
注释，
不会在浏览器中显示。
-->
```

### hack方法
hack方法就是利用markdown的解析原理来实现注释的。

一般有的markdown解析器不支持上面的注释方法，这个时候就可以用hack方法。

hack方法比上面2种方法稳定得多，但是语义化太差。

```
[comment]: <> (哈哈我是注释，不会在浏览器中显示。)
[comment]: <> (哈哈我是注释，不会在浏览器中显示。)
[comment]: <> (哈哈我是注释，不会在浏览器中显示。)
[//]: <> (哈哈我是注释，不会在浏览器中显示。)
[//]: # (哈哈我是注释，不会在浏览器中显示。)
```
其中，这种方法最稳定，适用性最强：
```
[//]: # (哈哈我是注释，不会在浏览器中显示。)
```
这种最可爱，超级无敌萌啊：
```
[^_^]: # (哈哈我是注释，不会在浏览器中显示。)
```
更为关键的是，这种方法最强，支持多行注释，
```
[^_^]:
    commentted-out contents
    should be shift to right by four spaces (`>>`).
```
### 示例测试

1.html标签（你如果看不到下面的注释说明已经成功注释）

2.html注释（你如果看不到下面的注释说明已经成功注释）

3.hack注释（你如果看不到下面的注释说明已经成功注释）

## Ref

- https://www.jianshu.com/p/9be87e7e15bf
- http://www.cnblogs.com/yangzhou33/p/8438461.html
- https://stackoverflow.com/questions/4823468/comments-in-markdown