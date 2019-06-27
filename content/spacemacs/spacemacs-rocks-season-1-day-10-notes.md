+++
title = "Spacemacs Rocks Day 10, spacemacs tutor"
date = 2018-11-21T00:00:00+08:00
lastmod = 2018-11-22T18:57:48+08:00
tags = ["emacs", "spacemacs", "tutor"]
categories = ["spacemacs"]
draft = false
weight = 3012
+++

[youku spacemacs tutor](http://v.youku.com/v%5Fshow/id%5FXMTI4MjY5ODIyOA==.html?spm=a2h0j.11185381.listitem%5Fpage1.5!5~A&&f=26137579)


## 安装 {#安装}


## 各文件夹功能 {#各文件夹功能}


### core {#core}

方便加载layer的配置


## 使用 proxychains4 打开 emacs {#使用-proxychains4-打开-emacs}

```
$ proxychains4 open /Applications/Emacs.app
```

这样的话，就会让emacs翻墙，这样下载melpa.org中的packages就会比较快。


## 介绍 spacemacs home page {#介绍-spacemacs-home-page}


## change-log {#change-log}


## Evil tutor {#evil-tutor}

> SPC h T


## universal arguments {#universal-arguments}

> SPC u


## 配置启动全屏 {#配置启动全屏}


## 在buffer中快速跳转 {#在buffer中快速跳转}

> M-x jump-in-buffer

现在有2种

> M-x helm-jump-in-buffer

和

> M-x counsel-jump-in-buffer


## 不需要安装layer，只单独安装某package {#不需要安装layer-只单独安装某package}

在 ~/.spacemacs.d/init.el 中的

dotspacemacs-additional-packages 中添加packages名就可以了。


## 行号 {#行号}

> SPC t n

相对行号

> SPC t r

这时，向下跳转9行，则只要：

> 9 j 或者 > C-u 9 j

同理，向上跳转9行，则只要：

> 9 k 或者 > C-u 9 k


## 中文输入法，chinese layer {#中文输入法-chinese-layer}

启动或退出（第一次运行时，要选择输入法）

> C-\\

切换输入法

> M-x set-input-method

查询输入法

> M-x list-input-method


## 跳转至 中文 {#跳转至-中文}

文中有 “东西” 2字，此时，想跳转到这里，则：

> M-x ace-pinyin-jump-word RTN dx RTN


## 创建layer {#创建layer}

> SPC : configuration-layer/create-layer

会选择一个目录地址，比如layer名为abc, 那直接回车，则会在

~/.spacemacs.d/layers/abc/ 下 自动生成 packages.el 文件


## 跳转到 layer 的文档或其中的某个package {#跳转到-layer-的文档或其中的某个package}

视频中的

> SPC f e h

修改成：

> SPC h SPC


## layer中配置package {#layer中配置package}

下以配置 zilongshanren-misc/packages.el 中的 helm-github-stats 为例子。

```
(defun zilongshanren-misc/init-helm-github-stars ()
  (use-package helm-github-stars
    :commands (helm-github-stars)
    :init
    （progn
      (setq helm-github-stars-username "secretrobots")
      (evil-leader/set-key
        "og" 'helm-github-stars) ）
    ))
```

其中，包括介绍了绑定leader-key。
