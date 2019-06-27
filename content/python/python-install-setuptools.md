---
title: "Python Install Setuptools"
date: 2018-06-04T14:29:49+08:00
draft: false
tags: ["python"]
categories: ["python"]
subtitle: "python源码安装setuptools"
descriptions: "源码安装 pip 时，出错，并提示` ImportError: No module named setuptools `解决方法"
bigimg:
---

源码安装 pip 时，出错，并提示` ImportError: No module named setuptools `解决方法

## Env

- python: 2.7.9

## Step

安装过程详见这篇博客：
http://www.ttlsa.com/python/how-to-install-and-use-pip-ttlsa/


安装后运行到：python setup.py install出现错误，错误图片如下所示：

```
[root@localhost pip-1.5.4]# python setup.py install 
Traceback (most recent call last): 
File “setup.py”, line 6, in 
from setuptools import setup, find_packages 
ImportError: No module named setuptools
```

解决方法
```
wget http://pypi.python.org/packages/source/s/setuptools/setuptools-0.6c11.tar.gz
tar zxvf setuptools-0.6c11.tar.gz
cd setuptools-0.6c11
python setup.py build
python setup.py install
```
之后就可以开心的使用pip了

## Ref

- https://blog.csdn.net/yangbodong22011/article/details/52456581