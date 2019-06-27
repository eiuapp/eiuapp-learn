+++
title = "使用hugo的jane主题时，categories 书写不规范"
date = 2018-10-04T11:23:00+08:00
lastmod = 2018-11-28T00:17:36+08:00
tags = ["hugo", "jane", "categories"]
categories = ["hugo"]
draft = false
weight = 3001
+++

<!--more-->


## categories 不是列表 {#categories-不是列表}

```shell
➜  tomtsang-rootsongjc-hugo git:(master) ✗ hugo server
Building sites … ERROR 2018/11/26 01:07:40 Error while rendering "page" in "posts/": template: _default/single.html:11:5: executing "_default/single.html" at <partial "head.html" ...>: error calling partial: template: partials/head.html:28:16: executing "partials/head.html" at <index ($.Site.Data.a...>: error calling index: value has type []string; should be string
                                                                                                                                                                                                                                                                                              ERROR 2018/11/26 01:07:40 Error while rendering "page" in "post/": template: post/single.html:14:21: executing "content" at <.>: range can't iterate over cloud-native
ERROR 2018/11/26 01:07:40 in .Render: Failed to execute template "post/summary.html": template: post/summary.html:11:19: executing "post/summary.html" at <.>: range can't iterate over cloud-native
                                                                                                                                                                                                                                                                                              Total in 687 ms
                                                                                                                                                                                                                                                                                              Error: Error building site: logged 3 error(s)
                                                                                                                                                                                                                                                                                              ➜  tomtsang-rootsongjc-hugo git:(master) ✗
```

看上面的提示，知道，是与：

-   某一个在 content/post 文件夹下的 page 相关
-   文档内容中包含了 "cloud-native" 字符串

所以搜索，知道，是在 文档的 header 部分

```
---
categories: "cloud-native"
---
```

而不是

```
---
categories: ["cloud-native"]
---
```

所以，出错了。

现在这个看起来，好像没有问题了


### 批量处理 {#批量处理}

```shell
➜  tomtsang-rootsongjc-hugo git:(master) ✗ grep "categories: \"" -rn ./content/
```

把所有出现的 categories 放在 categories.txt 中，执行下面shell，就可以了。

```shell
for str in `cat categories.txt`
do
    echo ${str}
    sed -i "s/categories: \"${str}\"/categories: \[\"${str}\"\]/g" `grep "categories: \"${str}\""  -rl ./../content/post/`
done
```


## author 要为 字符串 {#author-要为-字符串}

但是，过一会，又出错了

```shell
➜  tomtsang-rootsongjc-hugo git:(master) ✗ hugo serve

                           | ZH-CN
                           +------------------|-------+
                           Pages            |   336
                           Paginator pages  |     0
                           Non-page files   |     0
                           Static files     |    19
                           Processed images |     0
                           Aliases          |   122
                           Sitemaps         |     1
                           Cleaned          |     0

                           Total in 321 ms
                           Watching for changes in /Users/tomtsang/bitbucket/qqbb/tomtsang-rootsongjc-hugo/{content,static,themes}
                           Watching for config changes in /Users/tomtsang/bitbucket/qqbb/tomtsang-rootsongjc-hugo/config.toml
                           Serving pages from memory
                           Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
                           Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
                           Press Ctrl+C to stop

                           Change detected, rebuilding site
                           2018-11-26 16:36:04.511 +0800
                           Source changed "/Users/tomtsang/bitbucket/qqbb/tomtsang-rootsongjc-hugo/content/posts/test-aa-topic-post-title.md": WRITE
                           ERROR 2018/11/26 16:36:04 Error while rendering "page" in "posts/": template: _default/single.html:11:5: executing "_default/single.html" at <partial "head.html" ...>: error calling partial: template: partials/head.html:28:16: executing "partials/head.html" at <index ($.Site.Data.a...>: error calling index: value has type []string; should be string
                                                                                                                                                                                                                                                                                                        Total in 23 ms
                                                                                                                                                                                                                                                                                                        ERROR 2018/11/26 16:36:04 Failed to rebuild site: logged 1 error(s)

```

这下发现，只要一加入这个新的.md文件就出错了。

看\*.md文件，知道, header部分是这样的：

```
+++
title = "Post Title"
author = ["Bryce"]
date = 2017-12-19T17:00:00+08:00
lastmod = 2018-11-26T16:36:03+08:00
tags = ["post", "tags"]
categories = ["topic"]
draft = false
+++
```

其中categories格式正确，但是，这个 author 渲染不了。
如果此时，修改 author = ["Bryce"] 为 author = "Bryce" ，则能够正常渲染。

那怎么办？

1.  修改 author ，去除中括号
2.  去除 author 。

思路1。不容易，原因：
这个中括号，是由 ox-hugo 自动生成的，且，不容易修改 ox-hugo 的配置文件 \`~/.emacs.d/elpa/develop/org-plus-contrib-20181029/ox.el\`。

思路2。可以在 一级标题下的 PROPERTIES 中 加入 \`:EXPORT\_HUGO\_CUSTOM\_FRONT\_MATTER:
:noauthor true\` ，我们看一下效果。在不修改 ox-hugo 配置的情况下，会使用
\`author = ["tom tsang"] \` 。所以还是没有效果。

经过查询ox-hugo官方文档，得知，要在 org 文档的头部添加：

```
#+author:
#+hugo_custom_front_matter: :author "Bryce"
```

这个时候，export得到的 .md文件，会有 \`author = "Bryce"\` 。
如果不想要author, 则直接写成下面这样：

```
#+author:
#+hugo_custom_front_matter: :author ""
```

这个时候，导出的.md文件就没有author。


## Ref {#ref}

-   <https://ox-hugo.scripter.co/doc/author/#forcing-author-to-be-a-string--alternative>
