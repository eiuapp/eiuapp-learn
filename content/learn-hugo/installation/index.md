---
date: 2018-09-10T21:07:13+08:00
title: "index安装"
weight: 201
description : "详细介绍Hugo的安装"
---

## 准备工作

### 安装golang

安装hugo之前，先安装好golang。

目前新版本的hugo如0.54版本，需要golang最新的1.11版本支持。安装hugo前最好检查一下hugo对golang的版本要求。

## 安装

在[Hugo Releases](https://github.com/spf13/hugo/releases)页面下载对应操作系统版本的安装包。

## Linux安装

找到linux的安装包，对于 ubuntu 可以直接用 deb 文件：

- hugo_0.54_Linux-64bit.deb

deb文件直接安装即可。

```bash
sudo dpkg -i hugo_0.54_Linux-64bit.deb 
```

### Mac安装

mac 下安装最简单的方式是用brew命令

```bash
brew install hugo
```

也可以从下载页面下载到macos的安装包，解压后，将 hugo 可执行文件放在path路径下即可。

### 安装后设置

验证安装：

```bash
$ hugo version
Hugo Static Site Generator v0.54.0-B1A82C61 linux/amd64 BuildDate: 2019-02-01T09:40:34Z
```

为了方便使用，增加hugo server 命令的 alias： 

```bash
alias h="hugo server --disableFastRender"
```

## 自动发布

以下是jenkins自动生成并发布到nginx的简单脚本：

```bash
sh update_academic.sh

# clean 
cd /var/www/skyao/
# 删除所有文件和文件夹，但排除以"learning-"前缀开头的
rm -rf `ls | grep -v "learning-"`
cd /var/lib/jenkins/workspace/skyao.io/

# 需要明确指定baseUrl
hugo --baseUrl="https://skyao.io/" -d "/var/www/skyao/"
```

