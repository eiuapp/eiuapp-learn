+++
title = "go-tour-zh离线安装(本地安装)"
date = 2018-11-21T00:00:00+08:00
lastmod = 2018-11-22T16:25:15+08:00
tags = ["golang", "tour", "install"]
categories = ["golang"]
draft = false
weight = 3001
+++

## 中文文档 {#中文文档}

最近尝试学习golang，在某个网站（真忘了）上发现gotour是一款灰常叼的教程&指南，之后搜索发现有前辈给出了[本地安装离线gotour的方法](http://www.cnblogs.com/chijianqiang/archive/2012/11/19/gotour.html)，但实际安装过程中发现一些问题：

1.通过go get bitbucket.org/mikespook/go-tour-zh/gotour 命令安装时报错，提示missing Mercurial command，原来是先需要[安装Mercurial](https://studygolang.com/articles/714)；

```
➜  bitbucket git:(master) ✗ go get bitbucket.org/mikespook/go-tour-zh/gotour
go: missing Mercurial command. See https://golang.org/s/gogetcmd
package bitbucket.org/mikespook/go-tour-zh/gotour: exec: "hg": executable file not found in $PATH
➜  bitbucket git:(master) ✗ brew install mercurial
Updating Homebrew...
==> Downloading https://homebrew.bintray.com/bottles/mercurial-4.8.mojave.bottle.tar.gz
######################################################################## 100.0%
==> Pouring mercurial-4.8.mojave.bottle.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d

zsh completions have been installed to:
  /usr/local/share/zsh/site-functions
==> Summary
🍺  /usr/local/Cellar/mercurial/4.8: 618 files, 9.9MB
➜  bitbucket git:(master) ✗ hg clone https://bitbucket.org/mikespook/go-tour-zh
```

2.顺利用以上命令安装成功后，原贴说直接运行gotour命令即可通过<http://127.0.0.1:3999> 进行本地访问gotour，然而，bin目录下没有gotour... [解决方法](https://studygolang.com/articles/4768)

综上，本帖将整体安装过程梳理一遍（go语言安装及配置就不再赘诉了）

1.下载并安装Mercurial及Git

　　Mercurial：<https://www.mercurial-scm.org/wiki/Download>

　　Git：<https://git-scm.com/download/win>

　　（如无法访问请科学上网或百度寻找国内下载源）

2.用Mercurial下载gotour离线包，命令如下

　　hg clone <https://bitbucket.org/mikespook/go-tour-zh>

3.用Git下载goTools&goNet，命令如下

　　go get github.com/golang/net

　　go get github.com/golang/tools

4.调整文件路径，确保以上所下载的三个文件夹分别在以下路径中

　　gotour离线包：$GOPATH/src/bitbucket.org/mikespook

　　net：$GOPATH/src/golang.org/x/

　　tools：$GOPATH/src/golang.org/x/

　　（$GOPATH指代你自己定义的GOPATH路径，请参考[go语言环境配置](https://blog.csdn.net/hil2000/article/details/41261267)，需要注意的是，GOPATH下要建立src、bin、pkg三个文件夹）

5.在命令行中切换到$GOPATH/src/bitbucket.org/mikespook/go-tour-zh/gotour，执行以下命令

　　go install

6.至此，在$GOPATH/bin目录下已经生成gotour文件了，将其复制到$GOROOT/bin目录下

7.命令行中执行gotour，到浏览器中访问<http://127.0.0.1:3999> 就可以查看本地的gotour了


### Ref {#ref}

-   <http://www.cnblogs.com/Vulpers/p/5562586.html>
-   [构建离线Go编程指南——gotour](http://www.cnblogs.com/chijianqiang/archive/2012/11/19/gotour.html)
-   [golang - revel安装手记](https://studygolang.com/articles/714)
-   [go-tour-zh离线安装](https://studygolang.com/articles/4768)
-   [Go语言开发环境配置](https://blog.csdn.net/hil2000/article/details/41261267)


## 英文文档 {#英文文档}

```
➜  ~ git:(master) ✗ godoc -http=:6060
```
