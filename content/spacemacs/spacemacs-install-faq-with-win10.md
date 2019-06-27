+++
title = "在win10上安装spacemacs的常见问题"
date = 2019-02-12T00:00:00-08:00
lastmod = 2019-02-12T12:09:25-08:00
tags = ["emacs", "spacemacs"]
categories = ["spacemacs"]
draft = false
+++

在win10上安装spacemacs后，出现的问题

### **Warning** 提示如下：

```
Error (use-package): Cannot load highlight-global
Warning (magit): Magit no longer uses Magit-Popup.
It now uses Transient.
See https://emacsair.me/2019/02/14/transient-0.1.

However your configuration and/or some third-party package that
you use still depends on the `magit-popup' package.  But because
`magit' no longer depends on that, `package' has removed it from
your system.

If some package that you use still depends on `magit-popup' but
does not declare it as a dependency, then please contact its
maintainer about that and install `magit-popup' explicitly.

If you yourself use functions that are defined in `magit-popup'
in your configuration, then the next step depends on what you use
that for.

* If you use `magit-popup' to define your own popups but do not
  modify any of Magit's old popups, then you have to install
  `magit-popup' explicitly.  (You can also migrate to Transient,
  but there is no need to rush that.)

* If you add additional arguments and/or actions to Magit's popups,
  then you have to port that to modify the new "transients" instead.
  See https://github.com/magit/magit/wiki/Converting-popup-modifications-to-transient-modifications
```

### 打开 spaemacs 后，首页提示如下：

```
Errors:
- 
    An error occurred while installing font-lock+ (error: (error Invalid version syntax: ‘Command ’(git submodule sync --recursive)’ exited with non-zero status 128: Fetching origin
    HEAD is now at f2c1ddc no summary available
    fatal: 'submodule' appears to be a git command, but we were not
    able to execute it. Maybe git-submodule is broken?
    ’ (must start with a number)))
    
- 
    An error occurred while installing counsel-css (error: (error Invalid version syntax: ‘Command ’(git submodule sync --recursive)’ exited with non-zero status 128: Fetching origin
    HEAD is now at f2c1ddc no summary available
    fatal: 'submodule' appears to be a git command, but we were not
    able to execute it. Maybe git-submodule is broken?
    Fetching origin
    HEAD is now at 0536af0 v1.0.5 bump
    fatal: 'submodule' appears to be a git command, but we were not
    able to execute it. Maybe git-submodule is broken?
    ’ (must start with a number)))
    

Warnings:
- Cannot find any of the specified fonts (Source Code Pro)! Font settings may not
    be correct.      
- More than one init function found for package ac-ispell. Previous owner was
    auto-completion, replacing it with layer zilongshanren-tomtsang.      
- More than one init function found for package flycheck. Previous owner was
    syntax-checking, replacing it with layer zilongshanren-tomtsang.      
- More than one init function found for package ox-hugo. Previous owner was org,
    replacing it with layer zilongshanren-tomtsang.      
```

按道理，应该一个一个把这些报错信息，处理一下。


###  
## ref
- 