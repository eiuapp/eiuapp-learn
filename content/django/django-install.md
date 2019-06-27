---
title: "Django Install"
date: 2018-04-08T13:25:25+08:00
draft: true
tags: ["django", "install"]
categories: ["django"]
subtitle: ""
descriptions: ""
bigimg:
---

django 安装


## faq

### ImportError: No module named Django 

pip已安装成功Django,import时依然提示ImportError: No module named Django?

``` 
➜  Documents python
Python 3.6.4 |Anaconda, Inc.| (default, Jan 16 2018, 12:04:33)
[GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import django
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'django'
>>>
➜  Documents 
```

### 看一下安装了没有

```
➜  Documents pip install django
Requirement already satisfied: django in /usr/local/lib/python3.6/site-packages
Requirement already satisfied: pytz in /usr/local/lib/python3.6/site-packages (from django)
➜  Documents ls /usr/local/lib/python3.6/site-packages/django
__init__.py  __pycache__  bin          contrib      db           forms        middleware   template     test         utils
__main__.py  apps         conf         core         dispatch     http         shortcuts.py templatetags urls         views
➜  Documents 
```

`pip freeze`

```
➜  Documents pip freeze
Django==2.0.4
pbr==4.0.1
pytz==2018.3
six==1.11.0
stevedore==1.28.0
virtualenv==15.2.0
virtualenv-clone==0.3.0
virtualenvwrapper==4.8.2
➜  Documents
```

python下typehelp('modules'),显示可import模块并没有Django：
```
➜  Documents python
Python 3.6.4 |Anaconda, Inc.| (default, Jan 16 2018, 12:04:33)
[GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> help("modules")
```
看了一下, 返回中没有那个 django, 但是前文提示安装成功了,说明,我们的路径不对了

怀疑可能是pip安装的python版本与当前版本不匹配，于是查看：
hon下typehelp('modules'),显示可import模块并没有Django：

```
➜  ~ python
Python 3.6.4 |Anaconda, Inc.| (default, Jan 16 2018, 12:04:33)
[GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import sys
>>> print(sys.path)
['', '/Users/tom/anaconda3/lib/python36.zip', '/Users/tom/anaconda3/lib/python3.6', '/Users/tom/anaconda3/lib/python3.6/lib-dynload', '/Users/tom/anaconda3/lib/python3.6/site-packages', '/Users/tom/anaconda3/lib/python3.6/site-packages/aeosa']
>>>
```

看这里的第4个, 突然就明白了, 与我们当时 `pip install django` 时 安装到的包路径不同呀~

### 修改路径

我这里是修改成下面方式

```
➜  ~ which python
/usr/local/bin/python
➜  ~ ll /usr/local/bin/python
lrwxr-xr-x  1 tom  admin  31 Mar 29 16:41 /usr/local/bin/python@ -> /Users/tom/anaconda3/bin/python
➜  ~ which pip
/usr/local/bin/pip
➜  ~ ll /usr/local/bin/pip
lrwxr-xr-x  1 tom  admin  28 Apr  8 12:13 /usr/local/bin/pip@ -> /Users/tom/anaconda3/bin/pip
➜  ~
```
这个时候, 我们再次安装 Django, 就发现成功了.

```
>>> import django
>>> print(django.__version__)
2.0.4
>>>
```



## ref

- [pip已安装成功Django,import时依然提示ImportError: No module named Django?](https://segmentfault.com/q/1010000007500658)