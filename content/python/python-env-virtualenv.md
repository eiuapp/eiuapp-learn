---
title: "python搭建多个互不干扰的开发环境"
date: 2018-10-15T11:47:30+08:00
draft: false
tags: ["python"]
categories: ["python"]
subtitle: "使用virtualenv和virtualenvwrapper安装python3和python2的多个环境"
descriptions: ""
bigimg:
---

## Env

- os: mac

## Step

### 安装 python3 

[Mac安装Python2和Python3、pip2和pip3、ipython2和ipython3](https://www.jianshu.com/p/3701ff3399dd)

#### 一、Homebrew的安装及使用

1. Homebrew的安装
2. Homebrew的使用


```
安装软件：brew install 软件名，例：brew install wget
搜索软件：brew search 软件名，例：brew search wget
卸载软件：brew uninstall 软件名，例：brew uninstall wget
更新所有软件：brew update
⚠️通过 update 可以把包信息更新到最新，不过包更新是通过git命令，所以要先通过 brew install git 命令安装git。

安装git
更新具体软件：brew upgrade 软件名 ，例：brew upgrade git
显示已安装软件：brew list
查看软件信息：brew info／home 软件名 ，例：brew info git ／ brew home git
⚠️brew home指令是用浏览器打开官方网页查看软件信息

查看那些已安装的程序需要更新： brew outdated
显示包依赖：brew reps
```

#### 二、安装Python3和pip3

1. 安装Python3  

```
brew install python
```

2. 测试

安装pip2

Mac电脑本身自带Python2但是不带pip2，所以通过再次用brew安装Python2之后自然就携带pip2

1. 安装Python2 

```
brew install python@2 
```

2. 测试 

#### 三、安装ipython2和ipython3 (可不安装)

通过pip安装

1. 安装ipython3

```
pip3 install ipython 
```

2. 测试ipython3是否安装成功

```
ipython3
```

3. 安装ipython2


```
pip install ipython
```
4. 测试ipython2是否安装成功

```
ipython
```

### 使用 python3 安装 virtualenvs

```
which python3
which pip3
vi .zshrc ## 具体添加的内容见下文
source .zshrc 
pip3 install virtualenv virtualenvwrapper

## 建立python3环境
mkvirtualenv -p /usr/local/bin/python3  quant-py3
## 建立python2环境
mkvirtualenv -p /usr/local/bin/python2  quant-py2
```

```
tail .zshrc
    ## python virtualenvs
    export VIRTUALENVWRAPPER_PYTHON="$(command \which python3)"
    export WORKON_HOME=$HOME/.virtualenvs
    export PROJECT_HOME=$HOME/workspace
    source /usr/local/bin/virtualenvwrapper.sh
```

### 虚拟环境使用方法

mkvirtualenv zqxt：创建运行环境zqxt

workon zqxt: 工作在 zqxt 环境 或 从其它环境切换到 zqxt 环境

deactivate: 退出终端环境


其它的：

rmvirtualenv ENV：删除运行环境ENV

mkproject mic：创建mic项目和运行环境mic

mktmpenv：创建临时运行环境

lsvirtualenv: 列出可用的运行环境

lssitepackages: 列出当前环境安装了的包

创建的环境是独立的，互不干扰，无需sudo权限即可使用 pip 来进行包的管理。

### 示例

#### python2

```
➜  ~ mkvirtualenv quant
New python executable in /Users/tomtsang/.virtualenvs/quant/bin/python2.7
Also creating executable in /Users/tomtsang/.virtualenvs/quant/bin/python
Installing setuptools, pip, wheel...done.
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant/bin/predeactivate
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant/bin/postdeactivate
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant/bin/preactivate
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant/bin/postactivate
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant/bin/get_env_details
(quant) ➜  ~ python
Python 2.7.15 (default, Oct  2 2018, 11:47:18)
[GCC 4.2.1 Compatible Apple LLVM 10.0.0 (clang-1000.11.45.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
(quant) ➜  ~ which python
/Users/tomtsang/.virtualenvs/quant/bin/python
(quant) ➜  ~ which python2
/Users/tomtsang/.virtualenvs/quant/bin/python2
(quant) ➜  ~ which python3
/usr/local/bin/python3
(quant) ➜  ~ python3
Python 3.7.0 (default, Oct  2 2018, 09:20:07)
[Clang 10.0.0 (clang-1000.11.45.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
(quant) ➜  ~ lsvirtualenv
quant
=====


(quant) ➜  ~ deactivate
```

#### python3

```
➜  ~ mkvirtualenv -p /usr/local/bin/python3  quant-py3
Running virtualenv with interpreter /usr/local/bin/python3
Using base prefix '/usr/local/Cellar/python/3.7.0/Frameworks/Python.framework/Versions/3.7'
/usr/local/lib/python2.7/site-packages/virtualenv.py:1041: DeprecationWarning: the imp module is deprecated in favour of importlib; see the module's documentation for alternative uses
  import imp
New python executable in /Users/tomtsang/.virtualenvs/quant-py3/bin/python3.7
Also creating executable in /Users/tomtsang/.virtualenvs/quant-py3/bin/python
Installing setuptools, pip, wheel...done.
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant-py3/bin/predeactivate
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant-py3/bin/postdeactivate
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant-py3/bin/preactivate
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant-py3/bin/postactivate
virtualenvwrapper.user_scripts creating /Users/tomtsang/.virtualenvs/quant-py3/bin/get_env_details
(quant-py3) ➜  
(quant-py3) ➜  ~ which python2
/usr/local/bin/python2
(quant-py3) ➜  ~ which python3
/Users/tomtsang/.virtualenvs/quant-py3/bin/python3
(quant-py3) ➜  ~ which python
/Users/tomtsang/.virtualenvs/quant-py3/bin/python
(quant-py3) ➜  ~ python
Python 3.7.0 (default, Oct  2 2018, 09:20:07)
[Clang 10.0.0 (clang-1000.11.45.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
(quant-py3) ➜  ~ python3
Python 3.7.0 (default, Oct  2 2018, 09:20:07)
[Clang 10.0.0 (clang-1000.11.45.2)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
(quant-py3) ➜  ~
(quant-py3) ➜  ~ deactivate
```


## Ref

- [Mac安装Python2和Python3、pip2和pip3、ipython2和ipython3](https://www.jianshu.com/p/3701ff3399dd)
- https://code.ziqiangxuetang.com/django/django-install.html
- https://blog.csdn.net/shile/article/details/78648587
