---
title: "Github Images Management"
date: 2019-06-08T10:07:10+08:00
draft: true
tags: ["github"]
categories: ["github"]
subtitle: ""
descriptions: ""
bigimg:
---

## PicGo(推荐)

https://github.com/Molunerfinn/PicGo

https://molunerfinn.com/PicGo/

## 通过github的隐藏功能issue

当前Github上没有提供asset管理工具，但是利用Github中的issues特性可以进行CDN asset的管理，合理利用issues会使得上传文件到知识库特别简单，这些资料不会被commit/change跟踪，当在知识库中创建issues时，可以采用简单的drag-n-drop拖曳上传资源至repository

最大的点在于, 上传后, 直接拿到 markdown 形式地圵.

## 以raw形式上传图片至github

1，在github创建repository
2，利用github的windows客户端clone至本地
3，将需要上传的文件放入本地的文件夹
4，利用github的windows客户端commit
5，网页浏览复制图片URL，如https://github.com/zhanghlHUST/figureForMarkdown/blob/master/17_urllib_parse_urlparse.JPG，将URL中blob替换为raw,即https://github.com/zhanghlHUST/figureForMarkdown/raw/master/17_urllib_parse_urlparse.JPG

## gitPic


## ref

- [github做Markdown图床](https://www.jianshu.com/p/33eeacac3344)
- https://841809077.github.io/2018/10/25/%E4%BD%BF%E7%94%A8github%E5%81%9A%E5%9B%BE%E5%BA%8A.html
- http://jingpin.jikexueyuan.com/article/36279.html
- https://zzzzbw.cn/article/6
- https://841809077.github.io/2018/10/25/%E4%BD%BF%E7%94%A8github%E5%81%9A%E5%9B%BE%E5%BA%8A.html
