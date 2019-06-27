+++
title = "quickly create a new draft post"
date = 2018-11-12T09:20:00+08:00
lastmod = 2018-11-15T00:49:28+08:00
tags = ["org", "ox-hugo"]
categories = ["org"]
draft = false
+++

Of course I’m using a handy capture template, as provided by the ox-hugo docs. This lets me type \`C-c c h\` to quickly create a new draft post.

```
(with-eval-after-load 'org-capture
  (defun org-hugo-new-subtree-post-capture-template ()
    "Returns `org-capture' template string for new Hugo post.
See `org-capture-templates' for more information."
    (let* ((title (read-from-minibuffer "Post Title: ")) ;Prompt to enter the post title
           (fname (org-hugo-slug title)))
      (mapconcat #'identity
                 `(
                   ,(concat "* TODO " title)
                   ":PROPERTIES:"
                   ,(concat ":EXPORT_FILE_NAME: " fname)
                   ":END:"
                   "%?\n")        ;Place the cursor here finally
                 "\n")))

  (add-to-list 'org-capture-templates
               '("h"              ;`org-capture' binding + h
                 "Hugo post"
                 entry
                 ;; It is assumed that below file is present in `org-directory'
                 ;; and that it has a "Blog Ideas" heading. It can even be a
                 ;; symlink pointing to the actual location of all-posts.org!
                 (file+olp "all-posts.org" "Blog Ideas")
                 (function org-hugo-new-subtree-post-capture-template))))
)))

```

如果直接使用上面的配置，则运行之前，有2个条件：

-   在 \`org-directory\` 中有个 名为 all-posts.org 的文件
-   此文件中 有一个一级标题，为"Blog Ideas"

要先查\`org-directory\`在哪里（SPC h d v），默认为~/org。如果想设置成其它文件夹，请看[org-mode-set-org-directory](https://b.qqbb.app/posts/org-mode-set-org-directory/)

```shell
➜  hugo git:(master) ✗ head ~/org/all-posts.org
```

我的all-posts.org的头是下面这样的：

> ```
> #+HUGO_BASE_DIR: /Users/tomtsang/bitbucket/qqbb/tomtsang-rootsongjc-hugo/
> *  Blog Ideas
>   :PROPERTIES:
>   # :EXPORT_HUGO_CUSTOM_FRONT_MATTER: :noauthor true :nocomment true :nodate true :nopaging true :noread true
>   :EXPORT_HUGO_SECTION: posts
>   :EXPORT_HUGO_WEIGHT: auto
>   :END:
> ** TODO test-a-aritile-title
>    :PROPERTIES:
>    :EXPORT_FILE_NAME: test-a-file-name
>    :END:
>
>    This is a abc test!
> ```


## Ref {#ref}

-   <https://www.baty.net/2018/lets-try-using-ox-hugo-again/>
