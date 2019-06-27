
ubuntu 安装 pip

## env

os: ubuntu server 16.04

## step

```shell
root@ubuntu:~# sudo apt-get install python-pip python-dev build-essential
Reading package lists... Done
Building dependency tree       
Reading state information... Done
build-essential is already the newest version (12.1ubuntu2).
python-dev is already the newest version (2.7.12-1~16.04).
The following NEW packages will be installed:
  python-pip
0 upgraded, 1 newly installed, 0 to remove and 98 not upgraded.
Need to get 0 B/144 kB of archives.
After this operation, 635 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Selecting previously unselected package python-pip.
(Reading database ... 70623 files and directories currently installed.)
Preparing to unpack .../python-pip_8.1.1-2ubuntu0.4_all.deb ...
Unpacking python-pip (8.1.1-2ubuntu0.4) ...
Processing triggers for man-db (2.7.5-1) ...
Setting up python-pip (8.1.1-2ubuntu0.4) ...
root@ubuntu:~# which pip
/usr/local/bin/pip
root@ubuntu:~# pip -V
Traceback (most recent call last):
  File "/usr/bin/pip", line 9, in <module>
    from pip import main
ImportError: cannot import name main
root@ubuntu:~# sudo ln -s /usr/local/bin/pip /usr/bin/pip
ln: failed to create symbolic link '/usr/bin/pip': File exists
root@ubuntu:~# which pip
/usr/local/bin/pip
root@ubuntu:~# 
```

出现的问题，真的是搞不懂了，python2到 2020过期，难道连 pip 也一并都没有人维护了。真的是。

把这个 /usr/bin/pip 删除掉

```shell
root@ubuntu:~# rm /usr/bin/pip
root@ubuntu:~# sudo ln -s /usr/local/bin/pip /usr/bin/pip
root@ubuntu:~# pip -V
/usr/local/lib/python2.7/dist-packages/pip/_vendor/requests/__init__.py:83: RequestsDependencyWarning: Old version of cryptography ([1, 2, 3]) may cause slowdown.
  warnings.warn(warning, RequestsDependencyWarning)
pip 19.0 from /usr/local/lib/python2.7/dist-packages/pip (python 2.7)
root@ubuntu:~# 
```

pip search 一下。

```shell
root@ubuntu:~# pip search pysftp
/usr/local/lib/python2.7/dist-packages/pip/_vendor/requests/__init__.py:83: RequestsDependencyWarning: Old version of cryptography ([1, 2, 3]) may cause slowdown.
  warnings.warn(warning, RequestsDependencyWarning)
DEPRECATION: Python 2.7 will reach the end of its life on January 1st, 2020. Please upgrade your Python as Python 2.7 won't be maintained after that date. A future version of pip will drop support for Python 2.7.
pysftp (0.2.9)             - A friendly face on SFTP
cheesefactory-sftp (0.13)  - Wrapper for Pysftp.
root@ubuntu:~#
```
安装成功

## ref
- http://makaidong.com/hellojesson/127680_20223560.html

