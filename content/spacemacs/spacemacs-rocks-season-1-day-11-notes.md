+++
title = "Spacemacs Rocks Day 11, spacemacs use daily"
date = 2018-11-20T00:00:00+08:00
lastmod = 2018-11-22T19:01:29+08:00
tags = ["emacs", "spacemacs"]
categories = ["spacemacs"]
draft = false
weight = 3013
+++

[youku spacemacs](http://v.youku.com/v%5Fshow/id%5FXMTI1NzQxMzkzNg==.html?f=26137579)


## 开启 org-pomodoro {#开启-org-pomodoro}

> M-x org-pomodoro

或者

> , C p


## 插入chrome tab 且有title {#插入chrome-tab-且有title}

无title可以使用

> C-c l

但是视频中的是有title的，命令

> C-c G c

已经失效。


## 记笔记主要用 deft {#记笔记主要用-deft}


## 代码跳转至定义 {#代码跳转至定义}

> , g

返回到跳转前

> C-o


## python 语法检查错误列表 {#python-语法检查错误列表}

> SPC e L


## 查看函数文件 {#查看函数文件}

> , h h


## REPL环境 {#repl环境}


### 打开 {#打开}

> , s i


### 发送选中区块代码至 REPL环境 {#发送选中区块代码至-repl环境}

> , s r


## 在emacs中直接调用搜索引擎进行搜索 {#在emacs中直接调用搜索引擎进行搜索}

> SPC a /

输入 bing 就是bing搜索，google就是google搜索


## lispy {#lispy}

这一小节的具体内容，还是直接看 spacemacs-rocks season2 的第17节视频吧


### 代码块内部跳转 {#代码块内部跳转}

跳转至开头

> M-a

向下跳转

> M-f


## hotspots {#hotspots}

> M-x helm-hotspots 或者 SPC o o

这里的 github 指的是 github.com上的star的仓库，选中，回车，浏览器就会打开这个仓
库。


## markdown {#markdown}


### markdown-toc {#markdown-toc}

自动生成文档目录

> M-x markdown-toc-generate-toc 或者 , i t

但是，发现有问题：

-   有时生成出来的toc有时层级不对。
-   preview的时候，要去除前后两行删除行。


## emacs中启动markdown preview {#emacs中启动markdown-preview}


### zilongshanren/markdown-to-html {#zilongshanren-markdown-to-html}

开启

> , p

使用 grip 程序preview markdown-to-html 文件。子龙说，这里的预览效果与
github上的md文件的效果是一模一样的。


#### grip安装 {#grip安装}

<https://github.com/joeyespo/grip>

```
brew install grip
```


### Ref {#ref}

-   <https://emacs-china.org/t/topic/1549/17>


## 打开PDF文件 {#打开pdf文件}

dired buffer 下

> & RTN


## 2048 game {#2048-game}


## 播放音乐 {#播放音乐}


## magit {#magit}

Untrack

> S-k


## 建议 {#建议}

-   使用spacemacs的develop分支，而不是master分支，因为master分支bug很多
-   对使用的Layer下的packages的文档，应当了解，查看。从而把没有必要的package
    exclude掉，从而加速emacs,
-   dotspacemacs/config 函数是所有启动后，最后一个被调用的。但是，依然不建议这么做，
    而是新建立一个自己的layer.
-   QQ群 59134186(已失效)
