+++
title = "在win10上安装emacs"
date = 2019-02-12T00:00:00-08:00
lastmod = 2019-02-12T10:09:25-08:00
tags = ["emacs"]
categories = ["emacs"]
draft = false
+++

在win10上安装emacs

## env

- os: win10
- emacs: 26.1

## step

### 官网下载 emacs 包

官网
https://www.gnu.org/software/emacs/

下载地址
http://mirrors.nju.edu.cn/gnu/emacs/windows/emacs-26/emacs-26.1-x86_64.zip

### 放到程序相应位置

解压后得到 `emacs-26.1-x86_64` 文件夹，剪切至程序安装目录 `C:\Program Files\emacs-26.1-x86_64`

### 设置环境变量

设置系统环境变量

-  declare a environment variable named HOME pointing to your user directory C:\Users\<username>

![emacs-install-with-win10-set-home](https://res.cloudinary.com/dmtixvmgt/image/upload/v1549938865/emacs-install-with-win10-set-home_hjld5a.png)

- path中增加emacs\bin。

![emacs-install-with-win10-set-env-path](https://res.cloudinary.com/dmtixvmgt/image/upload/v1549938865/emacs-install-with-win10-set-env-path_hkkznc.png)


### 检查

`win-r`输入`cmd`

```shell
C:\Users\DELL>emacs --version
GNU Emacs 26.1
Copyright (C) 2018 Free Software Foundation, Inc.
GNU Emacs comes with ABSOLUTELY NO WARRANTY.
You may redistribute copies of GNU Emacs
under the terms of the GNU General Public License.
For more information about these matters, see the file named COPYING.

C:\Users\DELL>
```

表示安装成功，happy!

### 安装效果

最后在cmd中运行`emacs -nw`，查看安装效果。

```text
-nw    No window模式，以字符界面打开。
-Q     快速启动。
```

## ref
- https://www.douban.com/note/511848652/
- http://ftp.gnu.org/gnu/emacs/windows/