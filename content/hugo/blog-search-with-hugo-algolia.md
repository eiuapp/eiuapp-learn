+++
title = "使用 hugo-algolia 进行文章内容搜索"
date = 2018-12-13T10:29:00+08:00
lastmod = 2018-12-13T10:58:14+08:00
tags = ["hugo", "algolia"]
categories = ["hugo"]
draft = true
weight = 3002
+++

希望能通过 搜索 快速找到内容.

我猜是 algoliasearch 这个东西搜索的, 但是, 在 config.\*  , yarn.lock 中没有找到...当然只剩下 themes 这一个位置..

果然在这里面能找到...

问:

```
搜索的内容源怎么设置么? 不要搜索至超神那去...
```

超哥回复:

```
用的是云搜索，需要上传本地的index到algolia上
目前我是手动上传的一个json文件
```

好吧, 好好去折腾 \`algolia search\` 吧

哈哈,首先要确认在 public/ 中有找到一个 \`rootsongjc-hugo.json\` 文件, 其次, 在 deploy.sh 文件中就有提到了 hugo-algolia 和 rootsongjc-hugo.json 文件.


## 安装 hugo-algolia {#安装-hugo-algolia}

```shell
➜  github git:(master) ✗ npm install hugo-algolia -g
/usr/local/bin/hugo-algolia -> /usr/local/lib/node_modules/hugo-algolia/bin/index.js
+ hugo-algolia@1.2.13
added 55 packages from 67 contributors in 11.873s


   ╭───────────────────────────────────────────────────────────────╮
   │                                                               │
   │       New minor version of npm available! 6.2.0 → 6.4.1       │
   │   Changelog: https://github.com/npm/cli/releases/tag/v6.4.1   │
   │               Run npm install -g npm to update!               │
   │                                                               │
   ╰───────────────────────────────────────────────────────────────╯

➜  github git:(master) ✗ which hugo-algolia
/usr/local/bin/hugo-algolia
➜  github git:(master) ✗
```


## 配置 config.yaml {#配置-config-dot-yaml}

在必要的地方配置 config.yaml 文件(一般就是在blog的根目录)，且把 hugo-algolia 中的信息填充进去。

```shell
➜  blog git:(master) ✗ cat config.yaml
---
baseurl: "https://b.qqbb.app/"
DefaultContentLanguage: "zh-cn"
hasCJKLanguage: true
languageCode: "zh-cn"
title: "****"
theme: "beautifulhugo"
metaDataFormat: "yaml"
subtitle: "I can help you building financial freedom confidence"
dateFormat: "January 2, 2006"
description: "SRE🤘 AI🤘 Blockchain🤘 Cloud Native🤘 Big Data🤘 Fintech🤘 Quant"
algolia:
  index: "algolia-index"
  key: "*********************"
  appID: "U********0"
---

➜  blog git:(master) ✗
```


## 运行 hugo-algolia {#运行-hugo-algolia}

```shell
➜  blog git:(master) ✗ hugo-algolia

```

这里会有一个比较长时间的等待。
但是，等待了好久，也没有效果，这个是什么原因？待续...
