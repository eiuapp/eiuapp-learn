---
title: "Convert Rst to Gfm Document Style"
date: 2018-06-21T22:02:47+08:00
draft: false
tags: ["rst", "markdown", "gfm", "pandoc"]
categories: ["markdown"]
subtitle: "RST(reStructuredText) 转换为 MD(Markdown)"
descriptions: ""
bigimg:
---

RST(reStructuredText) 转换为 MD(Markdown)

## Env

- os: ubuntu16.04
- ip: 192.168.31.199


## Step

准备

### 安装 restbuilder

https://pythonhosted.org/sphinxcontrib-restbuilder/

```
pip install sphinxcontrib-restbuilder
```

### 安装 pandoc

https://pandoc.org/installing.html
https://github.com/jgm/pandoc/releases/latest

我这里选择二进制安装包

```
wget https://github.com/jgm/pandoc/releases/download/2.2.1/pandoc-2.2.1-1-amd64.deb
dpkg -i ./pandoc-2.2.1-1-amd64.deb
```


### 操作

生成 rst 文件：
```
make -e SPHINXOPTS="-D language='zh_CN'" rst
```
转换 rst 到 gfm或markdown 文件, 写成一个脚本。

```
root@ud-b:~# cat convert-doc-style.sh
#!/usr/bin/env bash

export pandoc="pandoc +RTS -V0 -RTS"

cd $1 
FILES="*.rst"
for f in $FILES
do
  filename="${f%.*}"
  echo "Converting '$f' to '$filename.md'"
  $pandoc "$f" -f rst -t gfm -o "$filename.md"
  #$pandoc "$f" -f rst -t markdown -o "$filename.md"
done
root@ud-b:~#
```

## Ref

- [翻译 RST(reStructuredText) 和转换为 MD(Markdown)](http://blog.mygraphql.com/wordpress/?p=141)
- [github/markup](https://github.com/github/markup)
- https://help.github.com/categories/writing-on-github/
- [Github Flavored Markdown介绍](https://www.jianshu.com/p/cfPxyr)
- [附录：轻量级标记语言](http://www.worldhello.net/gotgithub/appendix/markups.html)
- [GFM(GitHub Flavored Markdown)与标准Markdown的语法区别](http://www.cnblogs.com/36bian/p/7568015.html)
- [文档格式转化神器pandoc](https://blog.just4fun.site/document-factory-pandoc.html)