---
title: "Golang Gvm"
date: 2018-07-15T14:09:03+08:00
draft: false
tags: ["golang", "gvm"]
categories: ["golang"]
subtitle: "Go 语言多版本安装及管理利器 - GVM"
descriptions: ""
bigimg:
---


[Go 语言多版本安装及管理利器 - GVM](https://bingohuang.com/go-gvm/)

## Env

- golang


## Step

### 安装指定版本 golang

这里要科学上网

通过 gvm 安装指定 go版本

```
➜  ~ gvm install go1.10
Downloading Go source...

```
但是这个时候，没有指定你会使用哪个版本

```
➜  ~ gvm list

gvm gos (installed)

   go1.10
   system

➜  ~
```

通过 gvm 指定 默认使用版本

```
➜  ~ gvm use go1.10 --default
Now using version go1.10
➜  ~ gvm list

gvm gos (installed)

=> go1.10
   system

➜  ~ 
```

看一下go的环境吧

```
➜  ~ go env
GOARCH="amd64"
GOBIN=""
GOCACHE="/Users/tomtsang/Library/Caches/go-build"
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/tomtsang/.gvm/pkgsets/go1.10/global"
GORACE=""
GOROOT="/Users/tomtsang/.gvm/gos/go1.10"
GOTMPDIR=""
GOTOOLDIR="/Users/tomtsang/.gvm/gos/go1.10/pkg/tool/darwin_amd64"
GCCGO="gccgo"
CC="clang"
CXX="clang++"
CGO_ENABLED="1"
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/go-build321836390=/tmp/go-build -gno-record-gcc-switches -fno-common"
➜  ~
```

看，完美呀，基本的环境都自动设置好了。

### 用gvm管理Go项目的workspace

创建项目的workspace
我们来新建一个项目，名字叫baozou。项目目录放在~/goproj/下。

```
mkdir -p ~/goproj/baozou  
cd ~/goproj/baozou  
gvm pkgset create --local  
gvm pkgset use --local  
mkdir src  
```

在上面的命令中，我们创建了`baozou`项目的目录，进入目录后，使用`gvm pkgset create --local`命令，将目录设为一个`local`的`pkgset`，然后通过`gvm pkgset use --local`来使用它，这时，当前的环境变量的`GOPATH`为:

```
$HOME/goproj/baozou:$HOME/goproj/baozou/.gvm_local/pkgsets/system/local:$HOME/.gvm/pkgsets/system/global
```

可以看到，`GOPATH`已经被设置为`baozou`这个项目目录了，这时我们执行`go get`命令来下载第三方库的时候，会默认下载到`$HOME/goproj/baozou/src`目录下。

接下来，我们来创建真正用于代码管理的目录：

```
mkdir -p ~/goproj/baozou/src/baozou  
cd ~/goproj/baozou/src/baozou  
git init  
```
这里，我们用git来管理项目的软件版本。现在，我们可以在`~/goproj/baozou/src/baozou`这个目录下来编写代码了。


## Ref

- https://bingohuang.com/go-gvm/
- https://github.com/moovweb/gvm
- https://studygolang.com/articles/4788