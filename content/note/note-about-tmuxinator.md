+++
title = "tmuxinator（笔记）"
date = 2019-01-28T00:00:00-08:00
lastmod = 2019-01-29T19:29:10-08:00
tags = ["tmux", "note"]
categories = ["tmux"]
draft = false
weight = 3002
+++

tmux进阶之tmuxinator


## 在Tmuxinator中配置layout {#在tmuxinator中配置layout}

<https://blog.suisuijiang.com/tmux-course-terminal-manage/>

我们可能会需要指定窗格的排班规则，tmuxinator支持设置tmux中的5种默认layout样式：

```
even-horizontal
         Panes are spread out evenly from left to right across the window.

even-vertical
         Panes are spread evenly from top to bottom.

main-horizontal
         A large (main) pane is shown at the top of the window and the
         remaining panes are spread from left to right in the leftover
         space at the bottom.  Use the main-pane-height window option to
         specify the height of the top pane.

main-vertical
         Similar to main-horizontal but the large pane is placed on the
         left and the others spread from top to bottom along the right.
         See the main-pane-width window option.

tiled
         Panes are spread out as evenly as possible over the window in
         both rows and columns.
```

但是默认的5种样式不能让我们自由的设定每个窗格的大小，所以我们要使用自定义的
layout，在tmux中可以使用C-b + 方向键调整窗格的大小。
tmuxinator中可以直接使用tmux中的layout值，在终端输入tmux list-windows可以查看每个窗口的layout值，我们只需将这个值写入tmuxinator项目配置配置中的layout即可。


### 自定义layout {#自定义layout}

<https://stackoverflow.com/questions/9812000/specify-pane-percentage-in-tmuxinator-project/9976282#9976282>
<http://zuyunfei.com/2013/08/09/tmuxinator-best-mate-of-tmux/>


## html view tmux {#html-view-tmux}

<https://github.com/k3rni/tmuxinator>


## 运维利器-tmuxinator与glances {#运维利器-tmuxinator与glances}

运维经常需要同时查看各台windows/linux服务器的实时状态。有没有方便的方法能够同时查看状态，而且支持windows和linux呢？tmuxinator，tmux, 结合使用glances可以达到目的。

glances 是一款用于 Linux、BSD、windows 的开源命令行系统监视工具，它使用 Python 语言开发，能够监视 CPU、负载、内存、磁盘 I/O、网络流量、文件系统、系统温度等信息。glances可以以服务模式启动，以服务模式启动后，可以从任意一台linux客户端用glances命令连接到glances服务，从而显示服务器的状态，而达到从客户端远程监控服务器的目的。

tmux是linux下面的一个分屏幕，可以把你的屏幕分成多个工作区，同时进行不同的工作。而tmuxinator则让tmux可以按照预先的设置，同时在一个界面的多块区域分别运行指定的命令。这样我们可以利用tmuxinator同时运行多个glances命令, 监控多台服务器的状态。

glances: <https://github.com/nicolargo/glances>

tmuxinator: <https://github.com/tmuxinator/tmuxinator>

tmuxinator结合glances，可同时看到多台服务器上的预警和严重问题（严重问题，告警在glances中会显示红色，黄色）。 各台server启动glances server: glances -s 0.0.0.0， 找一台linux客户端可以同时显示多台服务器的状态。(glances可以监控windows/linux/mac的状态)

示例文件~/.tmuxinator/glances.yml的配置:

```yaml
name: glances
root: ~/

windows:
  - editor:
      #layout: main-vertical
      layout: tiled
      panes:
        - glances -c 127.0.0.1
        - glances -c 192.168.1.5
        - glances -c localhost
        - glances -c 127.0.0.1
        - glances -c 127.0.0.1
```

tmuxinator start glances，运行效果如图：

![figure](http://www.yiwusuozhi.com/blog/images/tmuxinator-glances.jpg)


## Ref {#ref}

-   [tmuxinator github README.md 中文](http://hao.jobbole.com/tmuxinator/#comments)
-   <https://github.com/tmuxinator/tmuxinator>
-   <https://blog.csdn.net/u014717036/article/details/60139776>
