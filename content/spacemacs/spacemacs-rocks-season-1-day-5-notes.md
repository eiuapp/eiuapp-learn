+++
title = "Spacemacs Rocks Day 5, Find, Search and replace"
date = 2018-11-15T11:00:00+08:00
lastmod = 2018-11-16T23:51:00+08:00
tags = ["emacs", "spacemacs", "search"]
categories = ["spacemacs"]
draft = false
weight = 3007
+++

## Evil search {#evil-search}

1.  Use / and \* to start search.
2.  Use n & N to search forward and backward


### 进入 iedit 批量修改 {#进入-iedit-批量修改}

> \* e

修改的时候，按S则删除原单词，修改完成后，按ESC，2次，退出iedit模式回到Normal模式。


### 选择匹配范围。 {#选择匹配范围}

多次按 r 切换匹配区域（当前buffer, 当前屏幕，当前函数）

> \* r

按 e 进入编辑模式，按 f 进入函数区域，按S开始修改。


### 清除搜索下的高亮：evil-search-clear-highlight {#清除搜索下的高亮-evil-search-clear-highlight}

> SPC s c


## Swiper {#swiper}

1.  C-s to start search.

搜索到结果后，C-j, C-k, C-n, C-p 可以跳转

1.  C-c C-o to enter ivy-occur

可以把当前的搜索项，罗列在另一个buffer中。


## Helm-swoop {#helm-swoop}

SPC s s/S to search search
C-c C-e to enter edit mode
SPC h l(helm-resume)


### 直接搜索光标处的单词 {#直接搜索光标处的单词}

> SPC s S


### 批量修改 {#批量修改}

> 搜索后，C-c C-e

只是打开另一个buffer, 没有做到 批量修改，至于后面提到的 C-x
C-s 保存修改结果，更无从谈起了。

正确的是什么方式？


### 开启上一次搜索项目，查询上一次搜索结果 {#开启上一次搜索项目-查询上一次搜索结果}

修正：

SPC h l runs the command ivy-spacemacs-help-layers, 查询layers help文档。不是视
频说的：开启上一次搜索项目，查询上一次搜索结果。


## Helm-imenu-or-semantic {#helm-imenu-or-semantic}

To search structured word.
SPC s l


## Occur to search and multiple editing {#occur-to-search-and-multiple-editing}

M-s o to use occur and e to enter edit mode. C-c to finish editing.
Edit search result one by one.
Edit search result with iedit and multiple cursor.


## Helm -ag {#helm-ag}

F3 to save search result, SPC s L to open last save search buffer.

这里的 F3 to save search result, SPC s L to open last save search buffer. 没有做到,谁知道呀？


## SPC s p to search project wide {#spc-s-p-to-search-project-wide}

C-c c-e


## SPC s f to search for files and certain suffix files. {#spc-s-f-to-search-for-files-and-certain-suffix-files-dot}


### 指定搜索某单个文件 {#指定搜索某单个文件}

> SPC s f 选择某文件


### 指定搜索某类型文件 {#指定搜索某类型文件}

> SPC u SPC s f 选择某文件夹，选择某文件类型


## SPC s b to search all the open buffer {#spc-s-b-to-search-all-the-open-buffer}


## SPC n f to narrow to function, SPC n w to restore {#spc-n-f-to-narrow-to-function-spc-n-w-to-restore}

After narrowing, you could use :%s/old/new/g to replace


### 方法1 {#方法1}

标记搜索内容后，搜索buffer

> SPC s E

再按 F ,缩小到只有当前函数。再按S,进行编辑。


### 方法2 {#方法2}

> SPC n f

-   替换1

> %s/A/B/g

-   替换1

> 光标处，\* e S, 编辑完成后，ESC ESC，返回到 normal 模式

替换完成后，

> SPC n w
