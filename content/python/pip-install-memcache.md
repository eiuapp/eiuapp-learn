
python2 通过 pip 安装 memcache

## env

os: ubuntu server 16.04
python: 2.7.12

## step

`pip install python-memcached` 而不是 `pip install memcache`


```shell
root@ubuntu:~# pip install memcache
/usr/local/lib/python2.7/dist-packages/pip/_vendor/requests/__init__.py:83: RequestsDependencyWarning: Old version of cryptography ([1, 2, 3]) may cause slowdown.
  warnings.warn(warning, RequestsDependencyWarning)
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7.
Collecting memcache
  Could not find a version that satisfies the requirement memcache (from versions: )
No matching distribution found for memcache
root@ubuntu:~# pip install python-memcached
/usr/local/lib/python2.7/dist-packages/pip/_vendor/requests/__init__.py:83: RequestsDependencyWarning: Old version of cryptography ([1, 2, 3]) may cause slowdown.
  warnings.warn(warning, RequestsDependencyWarning)
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7.
Collecting python-memcached
  Downloading https://files.pythonhosted.org/packages/f5/90/19d3908048f70c120ec66a39e61b92c253e834e6e895cd104ce5e46cbe53/python_memcached-1.59-py2.py3-none-any.whl
Requirement already satisfied: six>=1.4.0 in /usr/lib/python2.7/dist-packages (from python-memcached) (1.10.0)
Installing collected packages: python-memcached
Successfully installed python-memcached-1.59
root@ubuntu:~# 
```

### check

```shell
root@ubuntu:/etc/swift# python -c "import memcache; print(1+1);"
2
root@ubuntu:/etc/swift# 
```

## ref

- https://blog.csdn.net/zhaoyangjian724/article/details/69817640