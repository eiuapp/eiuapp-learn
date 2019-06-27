+++
title = "Spacemacs Rocks Day 2, Use Org-mode to Management Your Time"
date = 2017-12-19
lastmod = 2018-11-16T23:08:07+08:00
tags = ["emacs", "spacemacs"]
categories = ["spacemacs"]
draft = false
+++

进入 agenda

> C-c a

进入天，再进入周，再返回至天

> C-c a a 选中项目 Rtn w d

这样，就可以把它们绑定一下。那怎么绑定呢？

子龙山人的 \`org-agenda-dir\` 相关配置，在\`~/.spacemacs.d/layers/zilongshanren/config.el\`

```
org-agenda-dir "~/org-notes"
deft-dir "~/org-notes"
blog-admin-dir "~/zilongshanren.com"))
```

所以，这里要替换成自己的。
