---
title: "Gcc Rpm Install Offline"
date: 2018-03-02T17:29:20+08:00
draft: false
tags: ["centos","rpm","gcc"]
categories: ["centos"]
subtitle: ""
descriptions: ""
bigimg:
---

gcc rpm 安装包下载

安装`gcc`, [下载](https://pkgs.org/download/gcc)

```
[root@localhost jlch]# rpm -ivh gcc-4.8.5-16.el7_4.1.x86_64.rpm
warning: gcc-4.8.5-16.el7_4.1.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
error: Failed dependencies:
	cpp = 4.8.5-16.el7_4.1 is needed by gcc-4.8.5-16.el7_4.1.x86_64
	glibc-devel >= 2.2.90-12 is needed by gcc-4.8.5-16.el7_4.1.x86_64
	libgcc >= 4.8.5-16.el7_4.1 is needed by gcc-4.8.5-16.el7_4.1.x86_64
	libgomp = 4.8.5-16.el7_4.1 is needed by gcc-4.8.5-16.el7_4.1.x86_64
	libmpc.so.3()(64bit) is needed by gcc-4.8.5-16.el7_4.1.x86_64
	libmpfr.so.4()(64bit) is needed by gcc-4.8.5-16.el7_4.1.x86_64
[root@localhost jlch]#
```

所有的下载地址如下:

http://mirror.centos.org/centos/7/os/x86_64/Packages/libgcc-4.8.5-16.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/gcc-4.8.5-16.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/cpp-4.8.5-16.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/glibc-devel-2.17-196.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/libgomp-4.8.5-16.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/libmpc-1.0.1-3.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/mpfr-3.1.1-4.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/glibc-headers-2.17-196.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/kernel-headers-3.10.0-693.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/glibc-2.17-196.el7.x86_64.rpm
http://mirror.centos.org/centos/7/os/x86_64/Packages/glibc-common-2.17-196.el7.x86_64.rpm

```
[root@localhost gcc]# cat rpm.sh
#!/bin/bash
echo "start"
rpm_packages_all=""
for rpm_packages in `cat rpm.txt`
do
    echo ${rpm_packages}
    rpm_packages_all="${rpm_packages_all} ${rpm_packages}"
done
echo ${rpm_packages_all}
rpm -ivh ${rpm_packages_all}
echo "rpm -ivh, end"
echo `which gcc`
```

```
[root@localhost gcc]# cat rpm.txt
cpp-4.8.5-16.el7.x86_64.rpm
glibc-devel-2.17-196.el7.x86_64.rpm
libgcc-4.8.5-16.el7.x86_64.rpm
libgomp-4.8.5-16.el7.x86_64.rpm
libmpc-1.0.1-3.el7.x86_64.rpm
mpfr-3.1.1-4.el7.x86_64.rpm
glibc-headers-2.17-196.el7.x86_64.rpm
kernel-headers-3.10.0-693.el7.x86_64.rpm
glibc-2.17-196.el7.x86_64.rpm
glibc-common-2.17-196.el7.x86_64.rpm
gcc-4.8.5-16.el7.x86_64.rpm
```
