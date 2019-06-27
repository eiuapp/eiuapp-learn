---
title: "Virtualbox Install Ubuntu Desktop"
date: 2018-03-01T22:47:20+08:00
draft: false
tags: ["virtualbox", "ubuntu"]
categories: ["virtualbox"]
subtitle: ""
descriptions: ""
bigimg:
---

在 ubuntu desktop 下安装virtualbox

## env

在 ubuntu desktop(硬盘9)中安装 virtualbox 总是不能启动


## step


```
tom@ud-3-1:~$ which VirtualBox
/usr/bin/VirtualBox
```
看一下启动文件

```
tom@ud-3-1:~$ ll /usr/bin/VirtualBox 
lrwxrwxrwx 1 root root 4 3月  16  2017 /usr/bin/VirtualBox -> VBox*
tom@ud-3-1:~$ ll /usr/bin/VBox
-rwxr-xr-x 1 root root 4589 3月  16  2017 /usr/bin/VBox*
tom@ud-3-1:~$ VirtualBox 
WARNING: The vboxdrv kernel module is not loaded. Either there is no module
         available for the current kernel (4.10.0-28-generic) or it failed to
         load. Please recompile the kernel module and install it by

           sudo /sbin/vboxconfig

         You will not be able to start VMs until this problem is fixed.
VirtualBox: Error -10 in SUPR3HardenedMain!
VirtualBox: Effective UID is not root (euid=1000 egid=1000 uid=1000 gid=1000)

VirtualBox: Tip! It may help to reinstall VirtualBox.
tom@ud-3-1:~$ sudo chmod 4711 /usr/lib/virtualbox/VirtualBox
tom@ud-3-1:~$ VirtualBox 
WARNING: The vboxdrv kernel module is not loaded. Either there is no module
         available for the current kernel (4.10.0-28-generic) or it failed to
         load. Please recompile the kernel module and install it by

           sudo /sbin/vboxconfig

         You will not be able to start VMs until this problem is fixed.
VirtualBox: supR3HardenedMainGetTrustedMain: dlopen("/usr/lib/virtualbox/VirtualBox.so",) failed: libQt5X11Extras.so.5: cannot open shared object file: No such file or directory
```
好像问题转到了`libQt5X11Extras.so.5`这样的软件依赖上去了, 那么百度吧 
```
tom@ud-3-1:~$ ls /usr/lib/virtualbox/VirtualBox.so
/usr/lib/virtualbox/VirtualBox.so
```
安装依赖
```
tom@ud-3-1:~$ sudo apt-get install  libqt5x11extras5 libsdl1.2debian
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  libqt5x11extras5 libsdl1.2debian
0 upgraded, 2 newly installed, 0 to remove and 369 not upgraded.
2 not fully installed or removed.
Need to get 175 kB of archives.
After this operation, 519 kB of additional disk space will be used.
Get:1 http://mirrors.aliyun.com/ubuntu xenial/universe amd64 libqt5x11extras5 amd64 5.5.1-3build1 [7,876 B]
Get:2 http://mirrors.aliyun.com/ubuntu xenial/main amd64 libsdl1.2debian amd64 1.2.15+dfsg1-3 [168 kB]
Fetched 175 kB in 5s (33.6 kB/s)         
Selecting previously unselected package libqt5x11extras5:amd64.
(Reading database ... 179737 files and directories currently installed.)
Preparing to unpack .../libqt5x11extras5_5.5.1-3build1_amd64.deb ...
Unpacking libqt5x11extras5:amd64 (5.5.1-3build1) ...
Selecting previously unselected package libsdl1.2debian:amd64.
Preparing to unpack .../libsdl1.2debian_1.2.15+dfsg1-3_amd64.deb ...
Unpacking libsdl1.2debian:amd64 (1.2.15+dfsg1-3) ...
Processing triggers for libc-bin (2.23-0ubuntu9) ...
Setting up libqt5x11extras5:amd64 (5.5.1-3build1) ...
Setting up libsdl1.2debian:amd64 (1.2.15+dfsg1-3) ...
Setting up virtualbox-5.1 (5.1.18-114002~Ubuntu~xenial) ...
Adding group `vboxusers' (GID 129) ...
Done.
```
启动成功


## ref

- http://blog.chinaunix.net/uid-20680966-id-5031178.html
- http://blog.csdn.net/ipsecvpn/article/details/52175279
