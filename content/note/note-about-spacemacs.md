+++
title = "spacemacs(笔记)"
date = 2019-01-19T00:00:00-08:00
lastmod = 2019-01-23T23:40:16-08:00
tags = ["spacemacs"]
categories = ["spacemacs"]
draft = false
weight = 3001
+++

load-themes(SPC T s)
org-cycle(TAB)

spacemacs打开org文件时默认没有自动折行。想要自动折行，则运行：

M-x spacemacs/toggle-visual-line-navigation-on
现在发现下面这个也可以自动折行：

M-x toggle-truncate-lines
M-x toggle-truncate-lines-on
M-x toggle-truncate-lines-off

最后，其实都是查看 SPC h d v truncate-lines 这个变量的值是t 还是 nil
