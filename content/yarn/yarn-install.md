---
title: "Yarn Install"
date: 2018-02-24T09:20:55+08:00
draft: false
tags: ["yarn", "install"]
categories: ["yarn"]
subtitle: ""
descriptions: ""
bigimg:
---

## env

mac book air

## step

安装 yarn

```
➜  tomtsang-rootsongjc-cheatsheet git:(master) brew install yarn
Updating Homebrew...
==> Auto-updated Homebrew!
Updated 2 taps (caskroom/cask, homebrew/core).
==> Updated Formulae
abcmidi               braid                 cryptopp              exiftool              fdroidserver          glbinding             haxe                  pwntools              x265
alot                  cgrep                 erlang                fbi-servefiles        freexl                graphene              miniupnpc             ranger

==> Downloading https://yarnpkg.com/downloads/1.3.2/yarn-v1.3.2.tar.gz
==> Downloading from https://github.com/yarnpkg/yarn/releases/download/v1.3.2/yarn-v1.3.2.tar.gz
######################################################################## 100.0%
🍺  /usr/local/Cellar/yarn/1.3.2: 14 files, 3.9MB, built in 40 seconds
➜  tomtsang-rootsongjc-cheatsheet git:(master) 
```

使用, 安装工程中的依赖包

```
➜  tomtsang-rootsongjc-cheatsheet git:(master) yarn install
yarn install v1.3.2
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
warning " > eslint-plugin-flowtype@2.37.0" has unmet peer dependency "eslint@>=2.0.0".
[4/4] 📃  Building fresh packages...
✨  Done in 85.79s.
➜  tomtsang-rootsongjc-cheatsheet git:(master) w
```


## ref

- [Yarn 构建工具入门基础](https://www.jianshu.com/p/f1d96bdc545b)
- [yarn 中文网](https://yarn.bootcss.com/)
- [yarn 安装](https://yarn.bootcss.com/docs/install.html)
