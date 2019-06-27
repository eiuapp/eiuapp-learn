---
title: "Svn Install Centos"
date: 2018-02-03T14:22:47+08:00
draft: false
tags: ["svn", "install"]
categories: ["svn"]
subtitle: ""
descriptions: ""
bigimg:
---

在 centos 下 安装SVN

```
yum install subversion -y
svnserve --version
mkdir svn
cd svn
pwd
rm /root/svn
rm /root/svn -p
rm /root/svn/ -p
rm /root/svn/ 
cd
pwd
rm /root/svn/
cd /home
mkdir svn
cd svn
pwd
mkdir project
svnadmin create /home/svn/project/
cd project/
ls
cd conf/
ls
vi passwd 
vi authz 
vi svnserve.conf
svnserve -d -r /var/svn/svnrepos
svnserve -d -r /home/svn/project/
ps -aux | grep svn
cd home
cd /home
ls
vi test
ls
svn import test svn://192.168.31.184/home/svn/project/test -m "test" --force-log
vi /root/.subversion/servers
yum install openssl openssl-devel
svn import test svn://192.168.31.184/home/svn/project/test -m "test" --force-log
history
```