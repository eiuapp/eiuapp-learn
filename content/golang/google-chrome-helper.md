---
title: "Google Chrome Helper"
date: 2018-06-14T20:17:09+08:00
draft: false
tags: ["google", "chrome"]
categories: ["chrome"]
subtitle: "What Is Google Chrome Helper, and Why Is It Hogging My CPU Cycles?"
descriptions: ""
bigimg:
---

`google chrome helper`占用超多CPU资源

## Env
- os: mac 

看视频的同学要先做到科学上网

## Step

### 2014年前的chrome

如果能看视频的朋友，点[这里](https://www.youtube.com/watch?v=jD7y4R6VNXU)

有一篇[文章](https://www.wired.com/2014/10/google-chrome-helper/)提过这个问题，这里面提到的解决方案如下
```
First, shut down all your Chrome windows without quitting the program. In the Chrome menu, go to “Preferences,” scroll all the way down in the menu, and click on “Show advanced settings…” The first item in the expanded advanced settings list will be “Privacy,” and click on the “Content Settings” button right under that. About halfway down the content settings list is a “Plug-ins” entry, which will likely be set to “Run automatically.” Instead, select “Click to play.”
```

### 2017年后的chrome

但是，现在这个`Plug-ins`已经不在这个位置了，所以，现在不知道怎么去禁用`google chrome helper`了。

所以，我现在还是每发现一次就直接找到对应的进程，kill掉。



## Ref
- https://www.wired.com/2014/10/google-chrome-helper/
- https://www.youtube.com/watch?v=jD7y4R6VNXU