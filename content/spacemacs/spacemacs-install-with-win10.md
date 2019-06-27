+++
title = "在win10上安装spacemacs"
date = 2019-02-12T00:00:00-08:00
lastmod = 2019-02-12T12:09:25-08:00
tags = ["emacs", "spacemacs"]
categories = ["spacemacs"]
draft = false
+++

在win10上安装spacemacs

## env

- os: win10
- emacs: 26.1
- spacemacs: develop分支

## step

### 安装 emacs

参考[这里](./emacs-install-with-win10/)

### 备份

```shell
cd ~
mv .emacs.d .emacs.d.bak
mv .emacs .emacs.bak
```

### 官网下载 spacemacs 包

```shell
git clone https://github.com/syl20bnr/spacemacs -b develop ~/.emacs.d
```

### Launch Emacs

Spacemacs will automatically install the packages it requires. If you get an error regarding package downloads then you may try to disable the HTTPS protocol by starting Emacs with

```shell
emacs --insecure
```

第一次运行，请选择默认选择项目（1. vim; 2. 推荐）

### 设置private配置

备份或删除原有配置文件：

mv 
```shell
git clone https://github.com/$yourAccount/spacemacs-private ~/.spacemacs.d
```

如果愿意，可以使用我的配置

```shell
git clone https://github.com/eiuapp/spacemacs-private-win10-tl ~/.spacemacs.d
```


### 检查

最后在cmd中运行`emacs`，查看安装效果。

```text
-nw    No window模式，以字符界面打开。
-Q     快速启动。
```

### 快捷方式

打开emacs安装地址（比如：`C:\Program Files\emacs-26.1-x86_64\bin`）, 找到，`runemacs.exe` （不是`emacs.exe`）设置 快捷方式 到桌面

## ref
- https://www.douban.com/note/511848652/
- http://ftp.gnu.org/gnu/emacs/windows/