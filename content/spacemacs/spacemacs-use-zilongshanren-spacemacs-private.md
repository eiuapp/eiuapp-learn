---
title: "Spacemacs Use Zilongshanren Spacemacs Private"
date: 2018-08-28T23:53:02+08:00
draft: false
tags: ["spacemacs"]
categories: ["spacemacs"]
subtitle: "直接使用zilongshanren的sapcemacs配置"
descriptions: ""
bigimg:
---

想直接使用，zilongshanren的 配置

## Env

- os: mac os 10.13.3
- emacs: 26.1
- spacemacs： 0.200.13@26.1


## prepare

在使用前，我们可以安装一些软件

```
brew install emacs
brew install ispell
brew install trash
brew tap caskroom/fonts && brew cask install font-source-code-pro
brew install ctags-exuberant
```

## Step

现在直接就报了一个错误。见 https://github.com/zilongshanren/spacemacs-private/issues/180 

然后，这个时候，从各地方，获知，要用 spacemacs 源码的 devlop分支，而不是master分支。所以。要重新clone一下。

```
➜  .emacs.d git:(develop) pwd
/Users/tomtsang/.emacs.d
➜  .emacs.d git:(develop) git remote -v
origin	https://github.com/syl20bnr/spacemacs.git (fetch)
origin	https://github.com/syl20bnr/spacemacs.git (push)
➜  .emacs.d git:(develop) git branch -v
* develop c122eb6a0 lsp-layer configuration and building blocks for derived layers.
  master  c7a103a77 Fix line separation in README
➜  .emacs.d git:(develop)
```

当然，我们的 zilongshanren 的配置，也应该是 devlop 分支的。

```
➜  .spacemacs.d git:(develop) ✗ pwd
/Users/tomtsang/.spacemacs.d
➜  .spacemacs.d git:(develop) ✗ git remote -v
origin	https://github.com/zilongshanren/spacemacs-private (fetch)
origin	https://github.com/zilongshanren/spacemacs-private (push)
➜  .spacemacs.d git:(develop) ✗ git branch -v
* develop ea09142 Merge branch 'windows' into develop
➜  .spacemacs.d git:(develop) ✗
```

这样，重新启动spacemacs, 就可以使用了。

## QA

### 问题1

```
Cannot find any of the specified fonts (Source Code Pro)! Font settings may not be correct.
```

https://github.com/syl20bnr/spacemacs/issues/7919

https://github.com/adobe-fonts/source-code-pro

```
brew tap caskroom/fonts && brew cask install font-source-code-pro
```

好像，这个问题，已经OK了。

## Ref

- https://github.com/zilongshanren/spacemacs-private/issues/180 