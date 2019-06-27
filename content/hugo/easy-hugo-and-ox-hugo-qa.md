+++
title = "关于 hugo, easy-hugo, ox-hugo, spacemacs 集成相关的问题"
date = 2018-11-12T10:20:00+08:00
lastmod = 2018-11-17T00:30:09+08:00
tags = ["org", "ox-hugo", "easy-hugo", "sapcemacs"]
categories = ["org"]
draft = false
weight = 3001
+++

## ox-hugo，easy-hugo, spacemacs的集成 {#ox-hugo-easy-hugo-spacemacs的集成}


### ox-hugo 写文档 {#ox-hugo-写文档}

这里，在写文档的时候，配置好就可以了,无需修改 spacemacs的配置。

具体的配置，见 ox-hugo-example-2.org, aa.org, ox-hugo-example-1.org


### 导出至easy-hugo的content/xxx文件夹中 {#导出至easy-hugo的content-xxx文件夹中}

这一步，可以被 auto-save 函数替换了。

C-c C-e H H


### easy-hugo preview {#easy-hugo-preview}

C-c e p


## 用 easy-hugo preview 有时会因为缓存问题，而失真哟 {#用-easy-hugo-preview-有时会因为缓存问题-而失真哟}

当时有一个 pages/about.md 文件导致的失真。在右上角的Menu中始终存在。

解决方式，清缓存。


## EXPORT\_HUGO\_SECTION 失效 {#export-hugo-section-失效}

当 content 下，没有pages文件夹时，在 \*.org 的

```
:EXPORT_HUGO_SECTION: pages
```

会失效，且，.md 文件会在 easy-hugo 的配置项 easy-hugo-postdir（我这里设置的是 content/posts/）下 .


## 同时有 .org 和 .md 文件，.org 有效，.md 失效 {#同时有-dot-org-和-dot-md-文件-dot-org-有效-dot-md-失效}

当 content/posts/ 下同时有 .org 和 .md 文件，则 .org 文件有效，.md 失效
