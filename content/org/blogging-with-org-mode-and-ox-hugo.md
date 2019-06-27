+++
title = "使用org-mode和ox-hugo写文档"
date = 2018-11-12T09:20:00+08:00
lastmod = 2018-11-17T00:30:43+08:00
tags = ["easy-hugo", "org", "ox-hugo"]
categories = ["org"]
draft = false
weight = 3002
+++

## Env {#env}

-   os: mac
-   Emacs: 26.1
-   Spacemacs: 0.300.0
-   Org: 9.1.2
-   Hugo: v0.41
-   ox-hugo: 20181106.2350
-   prodigy: 20180511.938


## Step {#step}


### Part 1: Blog Setup {#part-1-blog-setup}


#### Download Hugo {#download-hugo}

```shell
pacman -S hugo
```


#### Initialize Project {#initialize-project}

```shell
hugo new site test-blog
cd test-blog
git init
```


#### Download a Theme (YMMV) {#download-a-theme--ymmv}

This seems like a clean, simple [theme](https://github.com/goodroot/hugo-classic).

You can use a submodule like this:

```shell
git submodule add https://github.com/goodroot/hugo-classic.git themes/hugo-classic
```

Or you can just clone it

```shell
mkdir themes/
git clone git@github.com:goodroot/hugo-classic.git themes/hugo-classic
```


#### Copy the necessary files from the theme {#copy-the-necessary-files-from-the-theme}

Warning: This will destroy your config.toml file, so, optionally back yours up.

```shell
cp -r themes/hugo-classic/exampleSite/* .
```


### Part 2: Emacs/Org-Mode/OX-Hugo integration {#part-2-emacs-org-mode-ox-hugo-integration}


#### Installing ox-hugo {#installing-ox-hugo}

Add the following to your .emacs or ~/.emacs.d/init.el file.

```
(use-package ox-hugo :ensure t :after ox)
```


#### Configuring Ox-hugo {#configuring-ox-hugo}

Create a new org-mode file to represent your blog. This file shouldlive in the root of the blog.

```shell
touch blog.org
```


#### Create your blog content in org-mode {#create-your-blog-content-in-org-mode}

Here’s a starter template for the blog.org file:

```
#+HUGO_BASE_DIR: .
* Pages
  :PROPERTIES:
  :EXPORT_HUGO_CUSTOM_FRONT_MATTER: :noauthor true :nocomment true :nodate true :nopaging true :noread true
  :EXPORT_HUGO_MENU: :menu main    # 把 .md文件显示在 menu 中. 所以这个一般情况注释掉
  :EXPORT_HUGO_TITLE: hmm
  :EXPORT_HUGO_SECTION: pages    # 把.md文件放在 content/pages/ 下
  :EXPORT_HUGO_WEIGHT: auto
  :END:
** About Page
   :PROPERTIES:
   :EXPORT_FILE_NAME: about   # 导出的.md 文件名为 about.md
   :END:
This is my about page.
* Posts
  :PROPERTIES:
  # :EXPORT_HUGO_CUSTOM_FRONT_MATTER: :noauthor true :nocomment true :nodate true :nopaging true :noread true
  :EXPORT_HUGO_MENU: :menu main
  :EXPORT_HUGO_SECTION: posts
  :EXPORT_HUGO_WEIGHT: auto
  :END:
** First Post
   :PROPERTIES:
   :EXPORT_FILE_NAME: some-file-name
   :EXPORT_DATE: <2018-03-19 Mon 22:08>   # 导出的时间
   :EXPORT_HUGO_DRAFT: true    # 是否为draft
   :END:
```


#### Export your content to hugo with ox-hugo {#export-your-content-to-hugo-with-ox-hugo}

When editing the blog.org file, export it through org-export-dispatch:

This will export all the content from the blog.org file into the hugo project.

```shell
C-c C-e H A
```

Sometimes you just want to export one post/page (aka subtree in org-mode terms):

```shell
C-c C-e H H
```


### Start the Server {#start-the-server}

At this point, you

```shell
hugo server -D
```


### Extras {#extras}


#### MathJax {#mathjax}

If you’d like to have nice equation support, check this link: <https://ox-hugo.scripter.co/doc/equations/>

Here’s an example of a LaTeX formatted equation: E=−J∑Ni=1sisi+1

Add this to the footer.html to get MathJax support:

```html
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
  displayAlign: "center",
  displayIndent: "0em",

  "HTML-CSS": { scale: 100,
  linebreaks: { automatic: "false" },
  webFont: "TeX"
  },
  SVG: {scale: 100,
  linebreaks: { automatic: "false" },
  font: "TeX"},
  NativeMML: {scale: 100},
  TeX: { equationNumbers: {autoNumber: "AMS"},
  MultLineWidth: "85%",
  TagSide: "right",
  TagIndent: ".8em"
  }
  });
</script>
<!-- https://gohugo.io/content-management/formats/#mathjax-with-hugo -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS_HTML"></script>
<!-- <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-MML-AM_CHTML"></script> -->

```


## Ref {#ref}

-   <https://gohugo.io/tools/editors/#emacs>
-   <https://www.shanesveller.com/blog/2018/02/13/blogging-with-org-mode-and-ox-hugo/>
-   <https://www.xianmin.org/post/ox-hugo/>
-   <https://lakedenman.com/blog/posts/bloggin-hugo-ox-hugo/>
-   <https://www.baty.net/2018/lets-try-using-ox-hugo-again/>
