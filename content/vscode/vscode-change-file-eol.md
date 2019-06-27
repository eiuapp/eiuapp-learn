---
title: "Vscode Change File Eol"
date: 2018-07-11T11:21:21+08:00
draft: false
tags: ["vscode"]
categories: ["vscode"]
subtitle: "vscode 编辑器如何在用户配置文件里修改 行尾序列 "
descriptions: ""
bigimg:
---
vscode 编辑器如何在用户配置文件里修改 行尾序列 
## Env

- vscode


## Step

文件，首选项，设置，搜索“files.eol”，然后编辑，然后就保存在用户设置或者工作区设置。

下次打开一个新的文件时，就是 LF(\n)结尾的换行符了。

对于已有的文件，想批量转换的，请使用：

另外一个方法是使用git-windows自带有dos2unix.exe,
在`git bash`中执行 `find . -type f -exec dos2unix {} \; `批量转换

## Ref

- https://segmentfault.com/q/1010000006886783/a-1020000006890519
- https://segmentfault.com/q/1010000011799577