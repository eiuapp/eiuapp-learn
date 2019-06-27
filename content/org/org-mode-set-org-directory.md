+++
title = "org-mode-set-org-directory"
date = 2018-11-13T14:00:00+08:00
tags = ["org"]
categories = ["org"]
draft = false
+++

## 做 软连接 {#做-软连接}

这个方法比较方便，不需要另外再去修改spacemacs或emacs配置。

```shell
➜  tomtsang-rootsongjc-hugo git:(master) ✗ echo `pwd`/org
/Users/tomtsang/bitbucket/qqbb/tomtsang-rootsongjc-hugo/org
➜  tomtsang-rootsongjc-hugo git:(master) ✗ ln -s `pwd`/org/ ~/org
➜  tomtsang-rootsongjc-hugo git:(master) ✗ l ~/org
lrwxr-xr-x 1 tomtsang 60 Nov 13 14:55 /Users/tomtsang/org -> /Users/tomtsang/bitbucket/qqbb/tomtsang-rootsongjc-hugo/org/
➜  tomtsang-rootsongjc-hugo git:(master) ✗ l ~/org/
total 48K
drwxr-xr-x 13 tomtsang  416 Nov 13 14:53 .
drwxr-xr-x 24 tomtsang  768 Nov 12 01:56 ..
-rw-r--r--  1 tomtsang 1.5K Nov 13 14:52 all-posts.org
-rw-r--r--  1 tomtsang  758 Nov 13 00:27 emacs-install-base-ubuntu.org
➜  tomtsang-rootsongjc-hugo git:(master) ✗
```
