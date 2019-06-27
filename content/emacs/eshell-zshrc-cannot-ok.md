+++
title = "eshell 中 没有完全加载 .zshrc 中的配置"
date = 2018-12-22T09:44:00+08:00
lastmod = 2018-12-22T10:11:40+08:00
tags = ["emacs", "eshell", "zsh"]
categories = ["emacs"]
draft = false
weight = 3001
+++

## Env {#env}

下面是终端上 zsh 的输出：


```
➜  ~ cat /etc/shells
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
➜  ~ echo $SHELL
/bin/zsh
➜  ~ which js-beautify
/usr/local/bin/js-beautify
➜  ~
```

下面是emacs中 eshell(\`SPC '\`开启)的输出：

```
Welcome to the Emacs shell

~/org:master*? λ echo /etc/shells
/etc/shells
~/org:master*? λ cat /etc/shells
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
~/org:master*? λ echo $SHELL
/bin/zsh
~/org:master*? λ which js-beautify
which: no js-beautify in (/Users/tomtsang/.cask/bin:/Users/tomtsang/.gvm/pkgsets/go1.10/global/bin:/Users/tomtsang/.gvm/gos/go1.10/bin:/Users/tomtsang/.gvm/pkgsets/go1.10/global/overlay/bin:/Users/tomtsang/.gvm/bin:/Users/tomtsang/.gvm/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Applications/Emacs.app/Contents/MacOS/bin-x86_64-10_10:/Applications/Emacs.app/Contents/MacOS/libexec-x86_64-10_10:/usr/local/go/bin)
~/org:master*? λ
```

导致在\*.js文件中，不能使用js-beautify(打开方式\`, =\`）格式化js代码，报错信息如下：

```
zsh:1: command not found: js-beautify
```

怎么办呢？


## Step {#step}

> SPC h SPC eshell

查看文档 ~/.emacs.d/layers/+tools/shell/README.org 知道：

原理：修改 shell-default-shell 从 eshell 到 shell

操作：在配置文件 ~/.spacemacs.d/init.el 中修改 shell layer中的配置.

```
(shell :variables shell-default-shell 'shell)
```


## Ref {#ref}

-   <https://www.jb51.net/LINUXjishu/247797.html>
