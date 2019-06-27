---
title: "Python Pip Install Ssl Module Not Available"
date: 2018-06-04T15:08:37+08:00
draft: false
tags: ["python", "pip"]
categories: ["python"]
subtitle: "centos中使用pip安装报错：pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available"
descriptions: ""
bigimg:
---

先看报错

```
[root@bitspace pip-10.0.1]# python -m pip install virtualenv virtualenvwrapper
pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.
Collecting virtualenv
  Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError("Can't connect to HTTPS URL because the SSL module is not available.",)': /simple/virtualenv/
  Retrying (Retry(total=3, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError("Can't connect to HTTPS URL because the SSL module is not available.",)': /simple/virtualenv/
  Retrying (Retry(total=2, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError("Can't connect to HTTPS URL because the SSL module is not available.",)': /simple/virtualenv/
  Retrying (Retry(total=1, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError("Can't connect to HTTPS URL because the SSL module is not available.",)': /simple/virtualenv/
  Retrying (Retry(total=0, connect=None, read=None, redirect=None, status=None)) after connection broken by 'SSLError("Can't connect to HTTPS URL because the SSL module is not available.",)': /simple/virtualenv/
  Could not fetch URL https://pypi.org/simple/virtualenv/: There was a problem confirming the ssl certificate: HTTPSConnectionPool(host='pypi.org', port=443): Max retries exceeded with url: /simple/virtualenv/ (Caused by SSLError("Can't connect to HTTPS URL because the SSL module is not available.",)) - skipping
  Could not find a version that satisfies the requirement virtualenv (from versions: )
No matching distribution found for virtualenv
pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.
Could not fetch URL https://pypi.org/simple/pip/: There was a problem confirming the ssl certificate: HTTPSConnectionPool(host='pypi.org', port=443): Max retries exceeded with url: /simple/pip/ (Caused by SSLError("Can't connect to HTTPS URL because the SSL module is not available.",)) - skipping
[root@bitspace pip-10.0.1]# 
```

## Env


- os: centos6.9
- python: 2.7.9
- pip: 1.10.1

## Step

原理：系统安装 openssl, 并重编译python

```
yum install openssl openssl-devel -y
```

python重新编译

```
cd ~/python/Python-2.7.9/
./configure --prefix=/usr/local/python27 && make && make install
cd ~/python/setuptools/setuptools-0.6c11/
python setup.py build && python setup.py install 
cd ~/python/pip-10.0.1/
python setup.py build && python setup.py install 
```

再安装virtualenv
```
python -m pip install virtualenv virtualenvwrapper
```

## Ref

- https://jingyan.baidu.com/article/cbf0e500475c042eab289362.html
- https://www.cnblogs.com/allan-king/p/5445879.html