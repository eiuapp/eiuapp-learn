+++
title = "hugo-server-faq"
date = 2017-12-19
lastmod = 2018-11-17T16:18:38+08:00
tags = ["hugo"]
categories = ["hugo"]
draft = false
weight = 3001
+++

## Error building site: EOF {#error-building-site-eof}

```shell
➜  tomtsang-rootsongjc-hugo git:(master) ✗ hugo server
Building sites … ERROR 2018/11/17 15:50:31 EOF for org/aaaa.org
Total in 108 ms
Error: Error building site: EOF
➜  tomtsang-rootsongjc-hugo git:(master) ✗
```

其实这个时候，查看一下 org/aaaa.org文件，没有内容，所以，删除了就可以 hugo server了的。

```shell
➜  tomtsang-rootsongjc-hugo git:(master) ✗ cat content/org/aaaa.org
#+TITLE: aaaa
#+DATE: 2018-11-16T23:38:13+08:00
#+PUBLISHDATE: 2018-11-16T23:38:13+08:00
#+DRAFT: nil
#+TAGS: nil, nil
#+DESCRIPTION: Short description
➜  tomtsang-rootsongjc-hugo git:(master) ✗ rm -rf ./content/org
➜  tomtsang-rootsongjc-hugo git:(master) ✗
```
