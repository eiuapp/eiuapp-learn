---
title: "Go Learn"
date: 2018-02-05T14:25:08+08:00
draft: false
tags: ["golang", "learn"]
categories: ["golang"]
subtitle: ""
descriptions: ""
bigimg:
---

## 资料导航

http://www.geeker8.com/go/41

## [菜鸟教程](http://www.runoob.com/go/go-tutorial.html)

已完成 

优

- 比较易读

缺点

- 没介绍 通道，go协程，等并发概念和原理

## [Go语言第一课](https://www.imooc.com/learn/345)

[代码](https://gitee.com/tomt/gowithme-golang-imooc-GOyuyandiyike)

已完成

优

- 比较易读
- 有代码测试

## [Go简易教程](https://github.com/songleo/the-little-go-book_ZH_CN/)

[代码](https://gitee.com/tomt/gowithme-golang-the-little-go-book_ZH_CN)

学习中

在 6.3 小节完全懵逼

#### 读后感受

整体来说，我感觉，本书内容较淺而对新手不易理解，知识结构展示混乱，且无完整的示例代码。读完了，没有感觉学到东西。

## [Go入门指南](https://github.com/Unknwon/the-way-to-go_ZH_CN)

[gomake](https://github.com/Unknwon/the-way-to-go_ZH_CN/blob/master/eBook/09.8.md)

[godoc](https://github.com/Unknwon/the-way-to-go_ZH_CN/blob/master/eBook/09.6.md)

[9.11 在 Go 程序中使用外部库](https://github.com/Unknwon/the-way-to-go_ZH_CN/blob/master/eBook/09.11.md)

学习在 13.2 小节, 停止....

## [Go并发编程实战](https://pan.baidu.com/disk/home#list/vmode=list&path=%2Fbook%2FCSDN)

[随书源码](https://github.com/hyper0x/goc2p)

学习中, 现在第3章

## [An Introduction to Programming in Go](http://www.golang-book.com/books/intro)

url

- http://www.golang-book.com/books/intro
- [pdf](http://www.golang-book.com/public/pdf/gobook.0.pdf)
- [中文繁体](http://golang-zhtw.netdpi.net/)

code

[随书源码](https://gitee.com/tomt/gowithme-golang-An-Introduction-to-Programming-in-Go)


## [golangbootcamp](http://www.golangbootcamp.com/)

## [官方文档-中文]

#### [官方中文-Go编程语言规范](https://go-zh.org/ref/spec.old)

## [golang快速入门](https://www.jianshu.com/p/54885c8af9f3)

done

chan 部分有错，其它整体很不错了，可以作为小手册，参考用。

## [go语言圣经（中文版）](https://yar999.gitbooks.io/gopl-zh/)

## gobyexample

done

https://gobyexample.com, 这里有大量使用常规使用案例
https://gobyexample.xgwang.me/channels.html 这个是中文翻译


#### Go 状态协程

https://gobyexample.xgwang.me/stateful-goroutines.html 看不懂

## [build-web-application-with-golan](https://github.com/astaxie/build-web-application-with-golang)

```shell
➜  Learning-Go-zh-cn git:(master) make
mmark -html -head inc/head.html -css inc/book.css learninggo.md > learninggo.html
panic: runtime error: invalid memory address or nil pointer dereference
[signal SIGSEGV: segmentation violation code=0x1 addr=0x48 pc=0x1134c83]

goroutine 1 [running]:
github.com/mmarkdown/mmark/render/mhtml.bibliographyItem(0x11bc980, 0xc42013fd50, 0xc42013f9d0, 0xc42013fd01)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/mmarkdown/mmark/render/mhtml/hook.go:102 +0x183
github.com/mmarkdown/mmark/render/mhtml.RenderHook(0x11bc980, 0xc42013fd50, 0x11be900, 0xc42013f9d0, 0x1, 0x0, 0xc4200df601)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/mmarkdown/mmark/render/mhtml/hook.go:37 +0x38a
github.com/gomarkdown/markdown/html.(*Renderer).RenderNode(0xc4200f6240, 0x11bc980, 0xc42013fd50, 0x11be900, 0xc42013f9d0, 0xc42019b901, 0x0)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/gomarkdown/markdown/html/renderer.go:914 +0x168c
github.com/gomarkdown/markdown.Render.func1(0x11be900, 0xc42013f9d0, 0x10f8d01, 0x0)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/gomarkdown/markdown/markdown.go:63 +0x61
github.com/gomarkdown/markdown/ast.NodeVisitorFunc.Visit(0xc4201952c0, 0x11be900, 0xc42013f9d0, 0x1, 0x0)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/gomarkdown/markdown/ast/node.go:547 +0x43
github.com/gomarkdown/markdown/ast.Walk(0x11be900, 0xc42013f9d0, 0x11bcc20, 0xc4201952c0, 0x0)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/gomarkdown/markdown/ast/node.go:519 +0x6c
github.com/gomarkdown/markdown/ast.Walk(0x11be8a0, 0xc42013fc70, 0x11bcc20, 0xc4201952c0, 0x0)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/gomarkdown/markdown/ast/node.go:530 +0x1b5
github.com/gomarkdown/markdown/ast.Walk(0x11bde80, 0xc42013f1f0, 0x11bcc20, 0xc4201952c0, 0x0)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/gomarkdown/markdown/ast/node.go:530 +0x1b5
github.com/gomarkdown/markdown/ast.Walk(0x11bde20, 0xc42005e300, 0x11bcc20, 0xc4201952c0, 0xc4201952c0)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/gomarkdown/markdown/ast/node.go:530 +0x1b5
github.com/gomarkdown/markdown/ast.WalkFunc(0x11bde20, 0xc42005e300, 0xc4201952c0)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/gomarkdown/markdown/ast/node.go:553 +0x4b
github.com/gomarkdown/markdown.Render(0x11bde20, 0xc42005e300, 0x11bd180, 0xc4200f6240, 0x11a0d56, 0x13, 0x119f8e9)
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/gomarkdown/markdown/markdown.go:62 +0xe1
main.main()
	/Users/tomtsang/.gvm/pkgsets/go1.10/global/src/github.com/mmarkdown/mmark/mmark.go:171 +0x694
make: *** [learninggo.html] Error 2
➜  Learning-Go-zh-cn git:(master)
```


## 线上执行环境

https://play.golang.org, 线上执行环境，可演示简单程序

## <学习GO语言>中文版

https://mikespook.com/learning-go/
## 常规使用案例
## Golang：有趣的 channel 应用

看得不是很明白

http://www.cnblogs.com/luckcs/articles/2588200.html
## Ref

- https://www.zhihu.com/question/30461290?sort=created
- https://github.com/dariubs/GoBooks
- https://studygolang.com/pkgdoc
