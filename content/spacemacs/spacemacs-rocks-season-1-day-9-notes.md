+++
title = "Spacemacs Rocks Day 9, spacemacs as a life style"
date = 2018-11-20T00:00:00+08:00
lastmod = 2018-11-20T10:23:16+08:00
tags = ["emacs", "spacemacs"]
categories = ["spacemacs"]
draft = false
weight = 3011
+++

[spacemacs as a life style](http://v.youku.com/v%5Fshow/id%5FXMTQwNTg2NTkzMg==.html)
   视频中的命令，还没有找到

进入不同的项目，使用layer切换：

> SPC l l

一般先进入 @org 查看当天的任务

只有单你，确切地需要功能时，才去写elisp函数，实现自动化。


## <span class="org-todo todo TODO">TODO</span> 在 day 周期下，如何把任务设置成 done? {#在-day-周期下-如何把任务设置成-done}


## <span class="org-todo todo TODO">TODO</span> 开启ERC {#开启erc}

在 SPC f e d 中安装 erc layer.


## <span class="org-todo todo TODO">TODO</span> 通过 ispell 语法检查 {#通过-ispell-语法检查}

experience


## [osx-dictionary](https://github.com/xuchunyang/osx-dictionary.el) 英英翻译 {#osx-dictionary-英英翻译}


### 光标处翻译 {#光标处翻译}

比如，我们现在有ball这个单词，把光标放在ball上（b,l上都可以）,然后

> M-x osx-dictionary-search-word-at-point

或者

> SPC x w d

然后，就给出了英文翻译，这时在这个临时buffer中有 s, o, r, q 提示，我们按r就有发音了。


### 翻译一个输入的单词 {#翻译一个输入的单词}

> M-x osx-dictionary-search-input
