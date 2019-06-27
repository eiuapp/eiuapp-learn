---
title: "Vsftp Add User"
date: 2018-05-30T09:59:54+08:00
draft: false
tags: ["vsftp", "ftp"]
categories: ["ftp"]
subtitle: "vsftp增加ftp用户"
descriptions: ""
bigimg:
---

## env

- os: centos6.9
- ftp: vsftpd
- ip: 10.88.88.22

## step

### 加用户 

加用户，为安全，禁止用户登录

```
[root@bitspace vsftpd]# useradd liuchao
[root@bitspace vsftpd]# passwd liuchao
Changing password for user liuchao.
New password: 
[root@bitspace vsftpd]# passwd liuchao
Changing password for user liuchao.
New password: 
BAD PASSWORD: it is WAY too short
BAD PASSWORD: is too simple
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@bitspace vsftpd]# vi /etc/passwd
[root@bitspace vsftpd]# tail -1 /etc/passwd
liuchao:x:507:507::/home/liuchao:/sbin/nologin
[root@bitspace vsftpd]# su - liuchao
This account is currently not available.
[root@bitspace vsftpd]# 
```

### 把用户名加入chroot_list文件


```
[root@bitspace vsftpd]# pwd
/etc/vsftpd
[root@bitspace vsftpd]# 
[root@bitspace vsftpd]# vi chroot_list 
[root@bitspace vsftpd]# cat chroot_list 
liuchao
ftpuser
hello
ftp
[root@bitspace vsftpd]# ls
chroot_list  ftpusers  user_list  vsftpd.conf  vsftpd_conf_migrate.sh
[root@bitspace vsftpd]# 
```

重启 vsftpd
```
[root@bitspace vsftpd]# service  vsftpd restart
Shutting down vsftpd:                                      [  OK  ]
Starting vsftpd for vsftpd:                                [  OK  ]
```


### 测试 

```
[root@bitspace vsftpd]# ls /home/
ftp  hello  jack  liuchao  lost+found
[root@bitspace vsftpd]# echo "dfadfds" >> /home/liuchao/123.txt
```