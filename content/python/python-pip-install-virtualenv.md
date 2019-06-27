---
title: "Python Pip Install Virtualenv"
date: 2018-06-04T15:56:51+08:00
draft: false
tags: ["python"]
categories: ["python"]
subtitle: "python源码安装pip后，安装virtualenv环境"
descriptions: ""
bigimg:
---

python源码安装pip后，安装virtualenv环境

## Step

源码安装`pip`后, `pip`会在 `/usr/local/python27/bin/` 下，且由`pip`安装的模块下的命令文件（如下文中的`virtualenvwrapper.sh`)，也会在这里哟。

### 环境变量加 pip

which pip

```
[root@bitspace ~]# tail -1 ~/.bashrc 
export PATH=$PATH:/usr/local/python27/bin
[root@bitspace ~]# which pip
/usr/local/python27/bin/pip
[root@bitspace ~]# 
```

### 环境变量加 virtualenvwrapper.sh

which virtualenvwrapper.sh

```
[root@bitspace ~]# which virtualenvwrapper.sh
/usr/local/python27/bin/virtualenvwrapper.sh
[root@bitspace ~]# 
```

加环境变量

```
[root@bitspace ~]# tail -3 ~/.bashrc 
export WORKON_HOME=$HOME/.virtualenvs
export PROJECT_HOME=$HOME/workspace
source /usr/local/python27/bin/virtualenvwrapper.sh
[root@bitspace ~]# 
```

这个时候，已经可以使用`mkvirtualenv`,`workon`等下列命令了（但是which会不成功的，一定要注意了）。

```
mkvirtualenv zqxt：创建运行环境zqxt
workon zqxt: 工作在 zqxt 环境 或 从其它环境切换到 zqxt 环境
deactivate: 退出终端环境
其它的：
rmvirtualenv ENV：删除运行环境ENV
mkproject mic：创建mic项目和运行环境mic
mktmpenv：创建临时运行环境
lsvirtualenv: 列出可用的运行环境
lssitepackages: 列出当前环境安装了的包
```
创建的环境是独立的，互不干扰，无需sudo权限即可使用 pip 来进行包的管理。

```
[root@bitspace ~]# which mkvirtualenv
/usr/bin/which: no mkvirtualenv in (/usr/lib64/qt-3.3/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin:/usr/local/python27/bin)
[root@bitspace ~]# mkvirtualenv aone
New python executable in /root/.virtualenvs/aone/bin/python
Installing setuptools, pip, wheel...done.
(aone) [root@bitspace ~]# 
```





## Ref

- https://code.ziqiangxuetang.com/django/django-install.html