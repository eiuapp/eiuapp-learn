+++
title = "Spacemacs Rocks Day 3, Emacs as a C/C++ IDE"
date = 2018-11-15T11:00:00+08:00
lastmod = 2018-11-20T14:51:34+08:00
tags = ["emacs", "spacemacs"]
categories = ["spacemacs"]
draft = false
weight = 3005
+++

[emacs-as-c-ide(youku)](http://v.youku.com/v%5Fshow/id%5FXMTM2OTM3MDkwMA==.html?spm=a2h0j.11185381.listitem%5Fpage1.5!28~A)

先看 github的 README.org

因为我并没有开发C/C++, 所以我这里只记录一下。


## 代码跳转 {#代码跳转}

tag 跳转

> , g d

精确 跳转

> , g g

还有一个

> , g a

返回到跳转前

> C-o


## 生成 .ycm\_extra\_conf.py 文件 {#生成-dot-ycm-extra-conf-dot-py-文件}


### <span class="org-todo todo TODO">TODO</span> 自动生成.ycm\_extra\_conf.py 文件的程序，会在其github中放出 {#自动生成-dot-ycm-extra-conf-dot-py-文件的程序-会在其github中放出}

```
python config_gen.py ~/Github/Spacemacs-rocks/c++-project
```


## 安装ycmd {#安装ycmd}

-   <https://github.com/Valloric/ycmd>


## Compile?? {#compile}

推荐使用shell


## Debugging?? {#debugging}

推荐使用 shell + lldb
