+++
title = "mac中python安装pyenv"
date = 2018-12-16T00:00:00+08:00
lastmod = 2018-12-22T08:44:22+08:00
tags = ["python", "pyenv"]
categories = ["python"]
draft = false
weight = 3002
+++

pyenv 用于同时管理多个python版本，它可以为每个工作目录设定使用指定的py版本，在不同目录下使用不同的版本可以同时开发不同版本的项目。


## Ubuntu安装 {#ubuntu安装}

```shell
➜  oscar git:(master) ✗ git clone https://github.com/pyenv/pyenv ~/.pyenv
Cloning into '/Users/tomtsang/.pyenv'...
remote: Enumerating objects: 63, done.
remote: Counting objects: 100% (63/63), done.
remote: Compressing objects: 100% (37/37), done.
remote: Total 16547 (delta 24), reused 50 (delta 20), pack-reused 16484
Receiving objects: 100% (16547/16547), 3.24 MiB | 497.00 KiB/s, done.
Resolving deltas: 100% (11211/11211), done.
➜  oscar git:(master) ✗ git clone https://github.com/pyenv/pyenv-virtualenv ~/.pyenv/plugins/pyenv-virtualenv
Cloning into '/Users/tomtsang/.pyenv/plugins/pyenv-virtualenv'...
remote: Enumerating objects: 2027, done.
remote: Total 2027 (delta 0), reused 0 (delta 0), pack-reused 2027
Receiving objects: 100% (2027/2027), 572.01 KiB | 340.00 KiB/s, done.
Resolving deltas: 100% (1384/1384), done.
➜  oscar git:(master) ✗
```

将PYENV\_ROOT和pyenv init加入bash的~/.bashrc（或zsh的~/.zshrc）

```shell
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```

激活pyenv

source ~/.bashrc（或zsh的\`~/.zshrc\`）


## Mac 安装 {#mac-安装}

更新brew

```shell
brew update
```

使用brew安装pyenv

```shell
brew install pyenv
```

配置环境变量

```shell
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```

或vim直接编辑

激活环境

```shell
source ~/.zshrc
```


## 检验 {#检验}

```shell
pyenv -v
```


### pyenv: no such command \`sh-activate\` {#pyenv-no-such-command-sh-activate}

如果这个时候，会出现 “pyenv: no such command \`sh-activate\`”错误，再安装
pyenv-virtualenv 就可以了。

```shell
➜  ~ git:(master) ✗ pyenv -v
pyenv 1.2.8
pyenv: no such command `sh-activate'
➜  ~ git:(master) ✗ brew install pyenv-virtualenv
Updating Homebrew...
==> Downloading https://github.com/pyenv/pyenv-virtualenv/archive/v1.1.3.tar.gz
==> Downloading from https://codeload.github.com/pyenv/pyenv-virtualenv/tar.gz/v1.1.3
######################################################################## 100.0%
==> ./install.sh
==> Caveats
To enable auto-activation add to your profile:
  if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
==> Summary
🍺  /usr/local/Cellar/pyenv-virtualenv/1.1.3: 20 files, 62.2KB, built in 12 seconds
➜  ~ git:(master) ✗ pyenv -v
pyenv 1.2.8
➜  ~ git:(master) ✗
```


### Ref {#ref}

-   <https://stackoverflow.com/questions/37152659/bash-environment-variable-setting-of-home-returns-pyenv-no-such-command-sh>


## 常用命令 {#常用命令}

```shell
pyenv install --list # 列出可安装版本
pyenv install <version> # 安装对应版本
pyenv install -v <version> # 安装对应版本，若发生错误，可以显示详细的错误信息
pyenv versions # 显示当前使用的python版本
pyenv which python # 显示当前python安装路径
pyenv global <version> # 设置默认Python版本
pyenv local <version> # 当前路径创建一个.python-version, 以后进入这个目录自动切换为该版本
pyenv shell <version> # 当前shell的session中启用某版本，优先级高于global 及 local
```

使用virtualenv

```shell
pyenv virtualenv env # 从默认版本创建虚拟环境
pyenv virtualenv 3.6.4 env-3.6.4 # 创建3.6.4版本的虚拟环境
pyenv activate env-3.6.4 # 激活 env-3.6.4 这个虚拟环境
pyenv deactivate # 停用当前的虚拟环境

# 自动激活
# 使用pyenv local 虚拟环境名
# 会把`虚拟环境名`写入当前目录的.python-version文件中
# 关闭自动激活 -> pyenv deactivate
# 启动自动激活 -> pyenv activate env-3.6.4
pyenv local env-3.6.4

pyenv uninstall env-3.6.4 # 删除 env-3.6.4 这个虚拟环境
```

实践过程中发现创建虚拟环境后会多出两个，但是可以不用管，好像没什么影响


## 安装3.7.0 {#安装3-dot-7-dot-0}


### 提示了" zlib not available " {#提示了-zlib-not-available}

```shell
➜  ~ git:(master) ✗ pyenv install 3.7.0
python-build: use openssl from homebrew
python-build: use readline from homebrew
Downloading Python-3.7.0.tar.xz...
-> https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
Installing Python-3.7.0...
python-build: use readline from homebrew

BUILD FAILED (OS X 10.14 using python-build 20180424)

Inspect or clean up the working tree at /var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216140437.92921
Results logged to /var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216140437.92921.log

Last 10 log lines:
  File "/private/var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216140437.92921/Python-3.7.0/Lib/ensurepip/__main__.py", line 5, in <module>
    sys.exit(ensurepip._main())
  File "/private/var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216140437.92921/Python-3.7.0/Lib/ensurepip/__init__.py", line 204, in _main
    default_pip=args.default_pip,
  File "/private/var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216140437.92921/Python-3.7.0/Lib/ensurepip/__init__.py", line 117, in _bootstrap
    return _run_pip(args + [p[0] for p in _PROJECTS], additional_paths)
  File "/private/var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216140437.92921/Python-3.7.0/Lib/ensurepip/__init__.py", line 27, in _run_pip
    import pip._internal
zipimport.ZipImportError: can't decompress data; zlib not available
make: *** [install] Error 1
➜  ~ git:(master) ✗
```

提示了" zlib not availabl", 安装 zlib 后继续吧。

```shell
➜  ~ git:(master) ✗ brew install zlib
Updating Homebrew...
==> Downloading https://homebrew.bintray.com/bottles/zlib-1.2.11.mojave.bottle.tar.gz
######################################################################## 100.0%
==> Pouring zlib-1.2.11.mojave.bottle.tar.gz
==> Caveats
zlib is keg-only, which means it was not symlinked into /usr/local,
because macOS already provides this software and installing another version in
parallel can cause all kinds of trouble.

For compilers to find zlib you may need to set:
  export LDFLAGS="-L/usr/local/opt/zlib/lib"
  export CPPFLAGS="-I/usr/local/opt/zlib/include"

For pkg-config to find zlib you may need to set:
  export PKG_CONFIG_PATH="/usr/local/opt/zlib/lib/pkgconfig"

==> Summary
🍺  /usr/local/Cellar/zlib/1.2.11: 12 files, 373KB
➜  ~ git:(master) ✗
```

这样安装完成后，还是会有机同的提示，所以我们要完成刚刚的3个export，之后再次尝试。

```shell
➜  ~ git:(master) ✗ pyenv install 3.7.0
python-build: use openssl from homebrew
python-build: use readline from homebrew
Downloading Python-3.7.0.tar.xz...
-> https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
Installing Python-3.7.0...
python-build: use readline from homebrew

BUILD FAILED (OS X 10.14 using python-build 20180424)

Inspect or clean up the working tree at /var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216141406.7026
Results logged to /var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216141406.7026.log

Last 10 log lines:
  File "/private/var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216141406.7026/Python-3.7.0/Lib/ensurepip/__main__.py", line 5, in <module>
    sys.exit(ensurepip._main())
  File "/private/var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216141406.7026/Python-3.7.0/Lib/ensurepip/__init__.py", line 204, in _main
    default_pip=args.default_pip,
  File "/private/var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216141406.7026/Python-3.7.0/Lib/ensurepip/__init__.py", line 117, in _bootstrap
    return _run_pip(args + [p[0] for p in _PROJECTS], additional_paths)
  File "/private/var/folders/pj/9t97fpn94r71_8631wx_x1hm0000gn/T/python-build.20181216141406.7026/Python-3.7.0/Lib/ensurepip/__init__.py", line 27, in _run_pip
    import pip._internal
zipimport.ZipImportError: can't decompress data; zlib not available
make: *** [install] Error 1
➜  ~ git:(master) ✗ which zlib
zlib not found
➜  ~ git:(master) ✗ export LDFLAGS="-L/usr/local/opt/zlib/lib"
➜  ~ git:(master) ✗ which zlib
zlib not found
➜  ~ git:(master) ✗ export CPPFLAGS="-I/usr/local/opt/zlib/include"
➜  ~ git:(master) ✗ export PKG_CONFIG_PATH="/usr/local/opt/zlib/lib/pkgconfig"
➜  ~ git:(master) ✗ which zlib
zlib not found
➜  ~ git:(master) ✗ pyenv install 3.7.0
python-build: use openssl from homebrew
python-build: use readline from homebrew
Downloading Python-3.7.0.tar.xz...
-> https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tar.xz
Installing Python-3.7.0...
python-build: use readline from homebrew
WARNING: The Python sqlite3 extension was not compiled. Missing the SQLite3 lib?
Installed Python-3.7.0 to /Users/tomtsang/.pyenv/versions/3.7.0

➜  ~ git:(master) ✗
```


## FAQ {#faq}


### 莫名其妙的BUILD FEILED (Ubuntu 16.04 using python-build 1.2.2) {#莫名其妙的build-feiled--ubuntu-16-dot-04-using-python-build-1-dot-2-dot-2}

问题是缺少依赖包，各个系统见以下链接

<https://github.com/pyenv/pyenv/wiki/Common-build-problems>


## 我的安装 {#我的安装}

```shell

# # if your system is ubuntu
# git clone https://github.com/yyuu/pyenv.git ~/.pyenv
# git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
# echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
# echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc

# # else if your system is mac
brew install openssl readline sqlite3 xz zlib
brew install pyenv
brew install pyenv-virtualenv

# 配置
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
# 激活配置
source ~/.zshrc
# # 判断有没有wget
command -v wget >/dev/null 2>&1 || { echo >&2 "I require wget but it's not installed.  Aborting."; exit 1; }
# # 不喜写兼容代码，所有代码均向 3.5+ 靠拢
v=3.7.0 | wget http://mirrors.sohu.com/python/$v/Python-$v.tar.xz -P ~/.pyenv/cache/;pyenv install $v
v=2.7.14 && wget http://mirrors.sohu.com/python/$v/Python-$v.tar.xz -P ~/.pyenv/cache/;pyenv install $v
# v=2.7.10 | wget http://mirrors.sohu.com/python/$v/Python-$v.tar.xz -P ~/.pyenv/cache/;pyenv install $v

# # 设置 Global Python 为 2.7.10, 备注：尽量不要把 Py3 设置为全局，否则由于 Homebrew 本身有一些应用是依赖于 Py2 的，设置为Py2容易出现一些奇怪的问题。
# pyenv global 2.7.14
pip install -i https://pypi.doubanio.com/simple requests
# 下面这个是用于安装基本的代码补全功能
pip install -i https://pypi.doubanio.com/simple --upgrade "jedi>=0.9.0" "json-rpc>=1.8.1" "service_factory>=0.1.5" flake8 pytest autoflake hy

pyenv virtualenv 3.7.0 py3-daily
pyenv activate py3-daily

# 安装依赖包
# pip install -i https://pypi.doubanio.com/simple requests
# pip install -i https://pypi.doubanio.com/simple beatutifulsoup4
# pip install -i https://pypi.doubanio.com/simple ipython[notebook]
# pip install -i https://pypi.doubanio.com/simple jupyter
# # 下面这个是用于安装基本的代码补全功能
# pip install -i https://pypi.doubanio.com/simple --upgrade "jedi>=0.9.0" "json-rpc>=1.8.1" "service_factory>=0.1.5" flake8 pytest autoflake hy
# 关闭自动激活,停用虚拟环境
# pyenv deactivate
# 卸载
# pyenv uninstall py3-daily
```


## Ref {#ref}

-   <https://www.jianshu.com/p/c5cc672aae63>
-   <https://www.jianshu.com/p/af1f8d7b6b31>
-   <https://www.jianshu.com/p/ad2cdcaef9e6>
-   <https://www.cnblogs.com/saneri/p/7642316.html>
-   <https://www.jianshu.com/p/5e00ca8a10d5>
-   <http://www.qingpingshan.com/m/view.php?aid=393729>
