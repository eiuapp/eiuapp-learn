+++
title = "shane sveller blog 笔记"
date = 2018-11-14T23:00:00+08:00
lastmod = 2018-11-19T01:10:33+08:00
tags = ["org", "ox-hugo"]
categories = ["org"]
draft = false
weight = 3004
+++

## notes {#notes}

[Blogging with org-mode and ox-hugo](https://www.shanesveller.com/blog/2018/02/13/blogging-with-org-mode-and-ox-hugo/) 这篇文章，写得很好，主要讲了下面几点。

1.  install
2.  Global settings and metadatad
3.  STARTUP: content
4.  AUTHOR: Shane Sveller
5.  HUGO\_BASE\_DIR: .
6.  HUGO\_AUTO\_SET\_LASTMOD: t
7.  Creating posts
8.  Setup hugo project category and tag.
9.  Excluding/heading sub-headings from export（it’s a :noexport: tag）
10. Automatic export on save
11. Marking a post as a Draft
12. Optional: Live reload without a separate shell tab
13. Emacs Lisp Snippets

其中几个说明一下。


### Automatic export on save {#automatic-export-on-save}

> ```
> * Footnotes
> * COMMENT Local Variables                                           :ARCHIVE:
>   # Local Variables:
>   # eval: (add-hook 'after-save-hook #'org-hugo-export-wim-to-md :append :local)
>   # eval: (auto-fill-mode 1)
>   # End:
> ```

其中，查一下函数（SPC h d f) 知道，要把 org-hugo-export-wim-to-md-after-save 修改成 org-hugo-export-wim-to-md。


### Marking a post as a Draft {#marking-a-post-as-a-draft}

把文章的标题，设置为 TODO 。就可以了。

所以，要忽略文章，或者说草稿部分，就2种方法:

-   文章：设置为 TODO
-   文章中的某部分：设置 :noexport: tag


### Optional: Live reload without a separate shell tab {#optional-live-reload-without-a-separate-shell-tab}

````
(prodigy-define-service
  :name "Hugo Server beautifulhugo"
  :command "/usr/local/bin/hugo"
  :args '("server" "-D" "--navigateToChanged" "-t" "beautifulhugo")
  :cwd "~/******jc-hugo"
  :tags '(hugo server)
  :stop-signal 'sigkill
  :init (lambda () (browse-url "http://localhost:1313"))
  :kill-process-buffer-on-stop t)
````

其中 Emacs Lisp Snippets 的功能，已经通过 \`:init (lambda () (browse-url
"<http://localhost:1313>"))\`实现了。


## Ref {#ref}

-   <https://www.shanesveller.com/blog/2018/02/13/blogging-with-org-mode-and-ox-hugo/>
-   <https://ox-hugo.scripter.co/doc/auto-export-on-saving/>
