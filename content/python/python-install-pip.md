---
title: "Python Install Pip"
date: 2018-06-04T14:22:12+08:00
draft: false
tags: ["python", "pip"]
categories: ["python"]
subtitle: "python详细安装pip教程"
descriptions: ""
bigimg:
---


## Env

- os: centos6.9
- python: 2.7.9

## step 

### 首先安装Python

我安装了两个版本:

　　Python-2.7.10.tgz

首先看一下系统自带的Python版本：
```
[root@zk src]# python -V
Python 2.6.6
```
安装Python2.7版本：

参考 [在centos中python从2.6升级到2.7](https://blog.finsoft.info/posts/python-upgrade-version-base-on-centos/)

### 开始安装pip

打开 https://pypi.org/search/?q=pip, 点击 `pip 10.0.1`， 点击 `Download files`, 右键`Copy link address`

#### 下载pip

```
[root@zk src]# wget "https://files.pythonhosted.org/packages/ae/e8/2340d46ecadb1692a1e455f13f75e596d4eab3d11a57446f08259dee8f02/pip-10.0.1.tar.gz"
[root@zk src]# tar -zxvf pip-1.5.4.tar.gz 
[root@zk src]# cd pip-1.5.4
[root@zk pip-1.5.4]# python setup.py install
```

如果出现下面错误
```
[root@zk pip-1.5.4]# python setup.py install
Traceback (most recent call last):
  File "setup.py", line 6, in <module>
    from setuptools import setup, find_packages
ImportError: No module named setuptools
```

看到`ImportError: No module named setuptools`，缺少setuptools模块

### 安装 setuptools

参考[python源码安装setuptools](https://blog.finsoft.info/posts/python-install-setuptools/), 下载安装setuptools模块：

```
[root@zk src]# wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-12.0.3.tar.gz#md5=f07e4b0f4c1c9368fcd980d888b29a65 
[root@zk src]# tar zxvf setuptools-12.0.3.tar.gz 
[root@zk setuptools-12.0.3]# python setup.py build
[root@zk setuptools-12.0.3]# python setup.py install
***
z = zipfile.ZipFile(zip_filename, mode, compression=compression)
  File "/usr/local/python27/lib/python2.7/zipfile.py", line 736, in __init__
    "Compression requires the (missing) zlib module"
RuntimeError: Compression requires the (missing) zlib module
```

看到缺少zlib模块,

### 安装 zlib

解决方法：

```
[root@zk setuptools-12.0.3]# yum install zlib zlib-devel
```

### 重新编译

安装完成之后需要重新编译Python2.7和3.5：

```
[root@zk setuptools-12.0.3]# cd ../Python-2.7.10
[root@zk Python-2.7.10]# ./configure --prefix=/usr/local/python27/
[root@zk Python-2.7.10]# make && make install
[root@zk Python-2.7.10]# rm -rf /usr/bin/python
[root@zk Python-2.7.10]# rm -rf /usr/bin/python3
[root@zk Python-2.7.10]# ln -s /usr/local/python27/bin/python /usr/bin/python
[root@zk Python-2.7.10]# cd ../setuptools-12.0.3
[root@zk setuptools-12.0.3]# python setup.py build
running build
running build_py
[root@zk setuptools-12.0.3]# python setup.py install
***
Processing dependencies for setuptools==12.0.3
Finished processing dependencies for setuptools==12.0.3
```

重新安装之后成功了！但是现在只是把setuptools安装好了，在重新安装pip：

```
[root@zk pip-1.5.4]# python setup.py install
Installed /usr/local/python27/lib/python2.7/site-packages/pip-1.5.4-py2.7.egg
Processing dependencies for pip==1.5.4
Finished processing dependencies for pip==1.5.4

[root@zk pip-1.5.4]# python -m pip install a

/usr/bin/python: cannot import name HTTPSHandler; 'pip' is a package and cannot be directly executed
```

根据上面提示又是缺少HTTPSHandler模块，安装：

### 安装 openssl与openssl-devel

```
[root@zk ~]# yum install openssl openssl-devel -y
```
然后再重新安装编译Python，安装完成时候在重新安装pip：

#### 查看一下pip版本

```
[root@bitspace pip-10.0.1]# python -m pip --version
pip 10.0.1 from /usr/local/python27/lib/python2.7/site-packages/pip-10.0.1-py2.7.egg/pip (python 2.7)
[root@bitspace pip-10.0.1]# 
```

#### 安装virtualenv 模块

```
[root@zk ~]# python
Python 2.7.10 (default, Apr 29 2016, 11:43:29) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-16)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import virtualenv
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named virtualenv
>>> exit()
[root@zk ~]# python -m pip install virtualenv
Downloading/unpacking virtualenv
  Downloading virtualenv-15.0.1-py2.py3-none-any.whl (1.8MB): 1.8MB downloaded
Installing collected packages: virtualenv
Successfully installed virtualenv
Cleaning up...
[root@zk ~]# python
Python 2.7.10 (default, Apr 29 2016, 11:43:29) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-16)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import virtualenv
>>> 
```

~ok,已经成功了！

如果安装的时候不出问题是最好的，所以在安装软件的时候一点要先把依赖包安装好！

## Ref

- https://www.cnblogs.com/allan-king/p/5445879.html