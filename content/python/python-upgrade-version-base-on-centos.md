---
title: "Python Upgrade Version Base on Centos"
date: 2018-06-04T10:06:58+08:00
draft: false
tags: ["python", "centos"]
categories: ["python"]
subtitle: "在centos中python从2.6升级到2.7"
descriptions: "CentOS6.9 升级 Python 2.7 版"
bigimg:
---

概要

CentOS 6.9中预安装了Python-2.6.6，其比较新的Python-2.7.9(CentOS 7预装版本)主要区别在于新版本的Python导入了更丰富的模块功能。对于初学者而言这一般不会有太大的影响，相对而言这些新模块在某些特定的编译环境下却是不可或缺的。例如：使用Devstack all-in-one模式进行安装OpenStack开发调试平台，需要Python-2.7及以上的支持，这样可以省去很多缺失模块的麻烦。 

## Env

- os: centos6.9
- python: 2.6.6 => 2.7.9

## step 

1.查看当前系统的Python Version 

```
[root@jmilk ~]# python --version
Python 2.6.6
```
2.下载Python-2.7.9

```
wget https://www.python.org/ftp/python/2.7.9/Python-2.7.9.tar.xz
```

3.安装Python 

a. 解压
```
tar -Jxvf Python-2.7.9.tar.xz -C /usr/src/
```
b. 安装

```
mkdir /usr/local/python27
cd /usr/src/Python-2.7.9/ 
./configure --prefix=/usr/local/python27 && make && make install
```

c. 将系统python指令默认指向Python-2.7.9版本 

CentOS6.9中YUM需要Python-2.6.6支持，所以不建议卸载老版本。

```
mv /usr/bin/python /usr/bin/python266
ln -s /usr/local/python27/bin/python2.7 /usr/bin/python
python --version 
```

### 解决YUM与Python2.7.9的兼容问题

因为YUM需要python-2.6.6的支持，CentOS 6.9中YUM却不兼容Python-2.7，导致YUM不可用。

```
[root@bitspace ~]# yum install python-pip -y
There was a problem importing one of the Python modules
required to run yum. The error leading to this problem was:

   No module named yum

Please install a package which provides this module, or
verify that the module is installed correctly.

It's possible that the above module doesn't match the
current version of Python, which is:
2.7.9 (default, Jun  4 2018, 18:00:05) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-18)]

If you cannot solve this problem yourself, please go to 
the yum faq at:
  http://yum.baseurl.org/wiki/Faq
  

```
需要在YUM的配置文件中，重新使YUM指向Python-2.6.6的执行程序(即CentOS 6.9 原始的/usr/bin/python程序)

```
vim /usr/bin/yum
```
将原来的：
```
#！/usr/bin/python
```
改为：
```
#!/usr/bin/python266
```
如下

```
[root@bitspace ~]# ls /usr/bin/python*
/usr/bin/python  /usr/bin/python2  /usr/bin/python2.6  /usr/bin/python266
[root@bitspace ~]# vi /usr/bin/yum
[root@bitspace ~]# yum install python-pip -y
```

一般来说这样就可以恢复使用YUM，同理所有在CentOS 6.5中对Python-2.7不兼容的软件都可以使用上面的方法来解决。

#### 如果上述步骤执行完后仍不能有效的执行YUN指令，可以尝试下面的解决方法。 

将CentOS 6.9的安装光盘或ISO文件中的以下rpm包(版本根据个人情况)拷贝到系统目录中。

```
mount /dev/cdrom /mnt/cdrom
 
cd /mnt/cdrom/Packages
cp yum-3.2.29-40.el6.centos.noarch.rpm \
python-2.6.6-51.el6.x86_64.rpm \
python-urlgrabber-3.9.1-9.el6.noarch.rpm \
python-devel-2.6.6-51.el6.x86_64.rpm \
python-libs-2.6.6-51.el6.x86_64.rpm /usr/local/python27
cd /usr/local/python27
rpm -Uvh --replacepkgs *.rpm
```
将上面的依赖包都安装完，或许可以解决这个问题。

至此，Python升级完成。

## Ref

- http://www.jb51.net/article/103967.htm
- https://www.cnblogs.com/allan-king/p/5445879.html