---
title: "Mac Iterm2 Ohmyzsh Install"
date: 2018-06-12T11:48:11+08:00
draft: false
tags: ["mac", "iterm2", "zsh"]
categories: ["mac"]
subtitle: "Mac下如何安装iTerm2并使用zsh iTerm2"
descriptions: ""
bigimg:
---

Mac下如何安装iTerm2并使用zsh iTerm2

## Env

- os: macOS

## Step

### 安装[iterm2](http://www.iterm2.com/)

安装iterm2只需要下载，解压安装就可以了。

### 安装[zsh](http://ohmyz.sh/)

#### 方式1
```
brew install zsh zsh-completions
```
无brew的请先安装Homebrew，mac上的一个包管理工具，很有必要安装，省去安装软件的麻烦。

#### 方式2
直接上官网查看安装方式

```
zsh --version
```
### 配置zsh
创建一个zsh的配置文件 
注意:如果你已经有一个~/.zshrc文件的话，建议你先做备份。使用以下命令
```
cp ~/.zshrc ~/.zshrc.orig
```

然后开始创建zsh的配置文件
```
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```
### 设置zsh为你的默认的shell
```
chsh -s /bin/zsh
```
然后退出iterm2，再重启就ok。

期间报的问题不要管直接按照步骤来就行了。

## Ref

- http://www.jianshu.com/p/77a4349bf67b
- https://blog.csdn.net/sufubo/article/details/54988457
