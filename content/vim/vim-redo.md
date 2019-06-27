+++
title = "vi撤销与恢复撤销"
date = 2018-11-12T18:31:00+08:00
lastmod = 2018-11-25T18:33:24+08:00
tags = ["vi", "undo"]
categories = ["vi"]
draft = false
weight = 3001
+++

## Step {#step}

在vi中按u可以撤销一次操作

```
  u   撤销上一步的操作
  Ctrl+r 恢复上一步被撤销的操作
```

注意：
如果你输入“u”两次，你的文本恢复原样，那应该是你的Vim被配置在Vi兼容模式了。
重做
如果你撤销得太多，你可以输入CTRL-R（redo）回退前一个命令。换句话说，它撤销一个撤销。
要看执行的例子，输入CTRL-R两次。字符A和它后面的空格就出现了：
young intelligent turtle

```
  有一个特殊版本的撤销命令：“U”（行撤销）。
```

行撤销命令撤销所有在前一个编辑行
上的操作。 输入这些命令两次取消前一个“U”：
A very intelligent turtle

```
  xxxx 删除very
```

A intelligent turtle

```
  xxxxxx 删除turtle
```

A intelligent

```
  用“U”恢复行
```

A very intelligent turtle

```
  用“u”撤销“U”
```

A intelligent

```
  “U”命令自己改变自己，“u”命令撤销操作，CTRL-R命令重做操作。这有点乱，但不用
```

担心，用“u”和CTRL-R命令你可以切换到任何状态。


## Ref {#ref}

-   <https://blog.csdn.net/xiongzhengxiang/article/details/7206691>
