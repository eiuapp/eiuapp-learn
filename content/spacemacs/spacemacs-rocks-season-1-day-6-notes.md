+++
title = "Spacemacs Rocks Day 6, Org-mode as a blogging engine"
date = 2018-11-16T11:00:00+08:00
lastmod = 2018-11-19T01:05:33+08:00
tags = ["emacs", "spacemacs"]
categories = ["spacemacs"]
draft = false
weight = 3008
+++

[youku](http://v.youku.com/v%5Fshow/id%5FXMTM4NzIyMTc0MA==.html?spm=a2h1n.8251843.playList.5!7~5~A&f=26137579&o=1)


## Start to blogging any place and any time. {#start-to-blogging-any-place-and-any-time-dot}

Press F1 to launch a menu and type `blog` to choose.
Navigation all the blog posts

<https://github.com/CodeFalling/blog-admin>

这里的配置，主要是针对 hexo


## Press W to new post {#press-w-to-new-post}

Enter the title of your new post

快速生成一个draft blog


### 下面这个在另一篇博文中已经提过。 {#下面这个在另一篇博文中已经提过}

> C-c t h


### 使用 yasnippet 模板，快速生成一个draft {#使用-yasnippet-模板-快速生成一个draft}

hugo 文章信息模板


## Start writing {#start-writing}

You could use all the Org syntax here. But org table is not well supported.

1.  Headings.
2.  Insert links.
3.  Insert Images
4.  Insert Code(We can even execute the code in the documents, oh my!)


### Insert links: {#insert-links}


#### zilongshanren/insert-chrome-current-tab-url {#zilongshanren-insert-chrome-current-tab-url}

> C-c l


### Insert Images {#insert-images}


#### 截图 {#截图}

> M-x zilongshanren/capture-screenshot

这里，子龙山人的操作，使得截图自动生成 ![](http://图床地址/图片名.png) ，其中图片名格式
为：日期-博文名-图片名.png, 且放置在了博客的一个图片文件夹中。
再通过一个 zilongshanren/octopress-upimg 函数，把这个图片上传到七牛云存储中，这
时回车，就可以通过浏览器访问到此图片。

这2步，他是怎么做到的呢？


### Insert Code {#insert-code}

> C-c i s

runs the command zilongshanren/org-insert-src-block ，插入完成后，
然后可以直接执行这一段代码。这个功能，还是蛮好的。

怎么执行的呢？

按 \`C-c\` 后，还按了什么键，出现了
\`Evaluate this C++ code block on your system? (y or n)\`

就是没有找到按的是什么键

但是我发现，只要把光标，移动到 \`#+BEGIN\_SRC\`这个地方，minibuffer处，会自动提示出
\`:results :exports :tangle\` 这些，然后，直接回车，就是上面这个询问。

```sh
echo "hello world"
```


## Preview and publish {#preview-and-publish}


### Preview {#preview}


#### runs the command prodigy to preview {#runs-the-command-prodigy-to-preview}

> SPC a S

然后按 s 启动 hugo server 就可以了。

-   s 开启服务
-   S 关闭服务
-   r 重启服务


#### prodigy的快捷键，参考这里 {#prodigy的快捷键-参考这里}

-   <https://segmentfault.com/a/1190000000452049>
-   <https://github.com/rejeep/prodigy.el>


#### 这里的前提是配置了hugo server 相关的 prodigy {#这里的前提是配置了hugo-server-相关的-prodigy}

参考 -
<https://www.shanesveller.com/blog/2018/02/13/blogging-with-org-mode-and-ox-hugo/>
或者子龙的hexo的配置就可以了。我的配置如下：

```
(prodigy-define-service
  :name "Hugo Server beautifulhugo"
  :command "/usr/local/bin/hugo"
  :args '("server" "-D" "--navigateToChanged" "-t" "beautifulhugo")
  :cwd "~/******jc-hugo"
  :tags '(hugo server)
  :stop-signal 'sigkill
  :init (lambda () (browse-url "http://localhost:1313"))
  :kill-process-buffer-on-stop t)

(prodigy-define-service
  :name "Hugo Server"
  :command "hugo"
  :args '("server")
  :cwd "~/******jc-hugo"
  :tags '(hugo server)
  :kill-signal 'sigkill
  :kill-process-buffer-on-stop t)

```

这里的 args 参数里的内容，可以自己先在 shell 下尝试，并了解清楚含义。比如 -t beautifulhugo指
的是theme主题名为beautifulhugo

```
➜  hugo git:(master) ✗ /usr/local/bin/hugo server -D --navigateToChanged -t beautifulhugo

                           | EN
                           +------------------|-----+
                           Pages            | 930
                           Paginator pages  |  66
                           Non-page files   |   0
                           Static files     |  28
                           Processed images |   0
                           Aliases          | 314
                           Sitemaps         |   1
                           Cleaned          |   0

                           Total in 632 ms
```


### deploy {#deploy}
