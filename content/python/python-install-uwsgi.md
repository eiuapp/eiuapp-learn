---
title: "Python Install Uwsgi"
date: 2018-04-17T23:51:55+08:00
draft: false
tags: ["pip", "python"]
categories: ["python"]
subtitle: ""
descriptions: ""
bigimg:
---

## env

- IP: home, 192.168.31.109
- OS: ubuntu, 16.04.03

## step

### `pip install uwsgi==2.0.17`时出错: `lto1: fatal error: bytecode stream generated with LTO version 6.0 instead of the expected 4.1`

```
(saleor-a-1) tom@saleor-a:~/saleor$ pip install uwsgi==2.0.17
_offload/offload.o plugins/router_memcached/router_memcached.o plugins/router_redis/router_redis.o plugins/router_hash/router_hash.o plugins/router_expires/expires.o plugins/router_metrics/plugin.o plugins/transformation_template/tt.o plugins/stats_pusher_socket/plugin.o -lpthread -lm -rdynamic -ldl -L/home/tom/anaconda3/lib -lpcre -L/home/tom/anaconda3/lib -lxml2 -L/home/tom/anaconda3/lib -lz -L/home/tom/anaconda3/lib -llzma -L/home/tom/anaconda3/lib -L/home/tom/anaconda3/lib -licui18n -licuuc -licudata -lm -ldl -lpthread -ldl -lutil -lrt -lm /home/tom/anaconda3/lib/python3.6/config-3.6m-x86_64-linux-gnu/libpython3.6m.a -lutil -lcrypt
    lto1: fatal error: bytecode stream generated with LTO version 6.0 instead of the expected 4.1
    compilation terminated.
    lto-wrapper: fatal error: gcc returned 1 exit status
    compilation terminated.
    /home/tom/anaconda3/compiler_compat/ld: error: lto-wrapper failed
    collect2: error: ld returned 1 exit status
    *** error linking uWSGI ***

    ----------------------------------------
Command "/home/tom/.virtualenvs/saleor-a-1/bin/python -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-it5mt8_t/uwsgi/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-record-dnakrkb4/install-record.txt --single-version-externally-managed --compile --install-headers /home/tom/.virtualenvs/saleor-a-1/include/site/python3.6/uwsgi" failed with error code 1 in /tmp/pip-install-it5mt8_t/uwsgi/
(saleor-a-1) tom@saleor-a:~/saleor$
```


> ---我是分割线---

后来, 在某天, 我又给找到了 [Ubuntu16.04上使用Anaconda3的Python3.6的pip安装UWSGI报错解决办法](https://www.cnblogs.com/liuzhen1995/p/8893962.html) 一文, 测试了一下. 成功了. 我了个去呀...

>原因是Ubuntu系统的gcc版本问题，我安装时本机的gcc版本是5.4，然后我把gcc版本修改成了4.7，重新使用`pip install uwsgi`，完美解决问题。
>
>安装gcc4.7:`sudo apt install gcc-4.7 -y`
>
>修改系统默认的gcc版本：https://blog.csdn.net/jacke121/article/details/54565281

操作一下

```
(saleor-a-1) tom@saleor-a:~/saleor$ sudo apt install gcc-4.7 -y
(saleor-a-1) tom@saleor-a:~/saleor$ ls /usr/bin/gcc* -l
lrwxrwxrwx 1 root root      5 Feb 11  2016 /usr/bin/gcc -> gcc-5
-rwxr-xr-x 1 root root 578880 Jan 19  2016 /usr/bin/gcc-4.7
-rwxr-xr-x 1 root root 915736 Feb  6 12:31 /usr/bin/gcc-5
lrwxrwxrwx 1 root root      8 Feb 11  2016 /usr/bin/gcc-ar -> gcc-ar-5
-rwxr-xr-x 1 root root  22912 Jan 19  2016 /usr/bin/gcc-ar-4.7
-rwxr-xr-x 1 root root  31136 Feb  6 12:31 /usr/bin/gcc-ar-5
lrwxrwxrwx 1 root root      8 Feb 11  2016 /usr/bin/gcc-nm -> gcc-nm-5
-rwxr-xr-x 1 root root  22912 Jan 19  2016 /usr/bin/gcc-nm-4.7
-rwxr-xr-x 1 root root  31136 Feb  6 12:31 /usr/bin/gcc-nm-5
lrwxrwxrwx 1 root root     12 Feb 11  2016 /usr/bin/gcc-ranlib -> gcc-ranlib-5
-rwxr-xr-x 1 root root  22912 Jan 19  2016 /usr/bin/gcc-ranlib-4.7
-rwxr-xr-x 1 root root  31136 Feb  6 12:31 /usr/bin/gcc-ranlib-5
(saleor-a-1) tom@saleor-a:~/saleor$ sudo rm -rf /usr/bin/gcc
(saleor-a-1) tom@saleor-a:~/saleor$ sudo ln -s /usr/bin/gcc-4.7 /usr/bin/gcc
(saleor-a-1) tom@saleor-a:~/saleor$ ls /usr/bin/gcc* -l
lrwxrwxrwx 1 root root     16 Apr 23 23:32 /usr/bin/gcc -> /usr/bin/gcc-4.7
-rwxr-xr-x 1 root root 578880 Jan 19  2016 /usr/bin/gcc-4.7
-rwxr-xr-x 1 root root 915736 Feb  6 12:31 /usr/bin/gcc-5
lrwxrwxrwx 1 root root      8 Feb 11  2016 /usr/bin/gcc-ar -> gcc-ar-5
-rwxr-xr-x 1 root root  22912 Jan 19  2016 /usr/bin/gcc-ar-4.7
-rwxr-xr-x 1 root root  31136 Feb  6 12:31 /usr/bin/gcc-ar-5
lrwxrwxrwx 1 root root      8 Feb 11  2016 /usr/bin/gcc-nm -> gcc-nm-5
-rwxr-xr-x 1 root root  22912 Jan 19  2016 /usr/bin/gcc-nm-4.7
-rwxr-xr-x 1 root root  31136 Feb  6 12:31 /usr/bin/gcc-nm-5
lrwxrwxrwx 1 root root     12 Feb 11  2016 /usr/bin/gcc-ranlib -> gcc-ranlib-5
-rwxr-xr-x 1 root root  22912 Jan 19  2016 /usr/bin/gcc-ranlib-4.7
-rwxr-xr-x 1 root root  31136 Feb  6 12:31 /usr/bin/gcc-ranlib-5
(saleor-a-1) tom@saleor-a:~/saleor$
```
再次安装 
```
(saleor-a-1) tom@saleor-a:~/saleor$ pip install uwsgi==2.0.17
```
成功了...

#### 下面是心路历程, 呵呵, 可忽略

这个问题, 找了下面几个网站, 但是, 还没有深入进去,

- https://techoverflow.net/2017/06/22/how-to-resolve-fatal-error-bytecode-stream-generated-with-lto-version-instead-of-the-expected/
- https://segmentfault.com/q/1010000002459736
- https://github.com/numenta/nupic/issues/2102
- https://github.com/numenta/nupic/issues/1699


后来,又 找到官网, 使用另一种安装方式作为替换(这个方法,没有成功)

效仿[官方WSGIquickstart](http://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html)安装方式之
```
curl https://uwsgi.it/install | bash -s pypy /tmp/uwsgi
```
而得到
```
curl https://uwsgi.it/install | bash -s pypy /home/tom/.virtualenvs/saleor-a-1/bin/uwsgi
```

但是, 还是没有安装成功.

### 经验

不认真看bug日志的程序员,不是一个好程序员...

## ref

- [Ubuntu16.04上使用Anaconda3的Python3.6的pip安装UWSGI报错解决办法](https://www.cnblogs.com/liuzhen1995/p/8893962.html)
- [官方WSGIquickstart](http://uwsgi-docs.readthedocs.io/en/latest/WSGIquickstart.html)