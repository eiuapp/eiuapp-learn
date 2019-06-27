+++
title = "emacs下使用hugo写文档"
date = 2018-11-12T20:00:00+08:00
draft = false
+++

emacs下使用hugo写文档, 主要有以下几种方式

无论是写哪种文档，都建议使用easy-mode创建,管理文档


## 写org文档 {#写org文档}


### 用ox-hugo写org文档 {#用ox-hugo写org文档}


#### 安装 {#安装}


#### 使用 {#使用}


### 用org-mode写org文档 {#用org-mode写org文档}


#### <span class="org-todo todo TODO">TODO</span> 直接用easy-mode创建新的.org文档，header不正常 {#直接用easy-mode创建新的-dot-org文档-header不正常}

看了一下代码，好像是源码里写死，没有使用到 archetype/default.org文件呀。

-    直接用easy-mode preview

    可以看到，


    TODO 避免删除后重新安装

    这样子的信息。


#### 导出成markdown {#导出成markdown}

发现写成org文档后，导出成markdown，会有content-table,这样，会有2个大纲，且，文章的时间不对。


#### 直接导出成html {#直接导出成html}

这时，没有大纲。但是，其它都还正常。


### <span class="org-todo todo TODO">TODO</span> Orgmode利用ox-pandoc导出成markdown {#orgmode利用ox-pandoc导出成markdown}

[Orgmode利用ox-pandoc导出hugo博客的workflow](https://blog.yuantops.com/tech/emacs-orgmode-hugo-with-oxpandoc/)


## 写markdown文档 {#写markdown文档}


### 用markdown-mode写markdown文档 {#用markdown-mode写markdown文档}
