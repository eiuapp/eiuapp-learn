---
title: "Centos6 Install Ftp"
date: 2018-05-29T10:39:09+08:00
draft: false
tags: ["centos","install", "ftp"]
categories: ["centos"]
subtitle: "CentOS6上ftp服务器搭建实战"
descriptions: ""
bigimg:
---

CentOS6上ftp服务器搭建实战

## env

- centos6.9
- IP: 10.88.88.22
- IP: 172.16.55.6

## step

### 1.安装程序包

```
[root@node1 ~]$ yum install -y vsftpd
[root@node1 ~]$ yum install -y lftp                   # 安装测试软件
```
### 2.启动vsftpd服务

```
[root@node1 ~]$ setenforce 0　　　　　　　　　　　　　　　　　　　　　　　#关闭selinux
setenforce: SELinux is disabled　　　　　　　　　　　　　　　　　　　　　
[root@node1 ~]$ service iptables stop　　　　　　　　　　　　　　　　　　#关闭防火墙　　
[root@node1 ~]$ service vsftpd start　　　　　　　　　　　　　　　　　　# 启动服务
为 vsftpd 启动 vsftpd：                                    [确定]
[root@node1 ~]$ service vsftpd status
vsftpd (pid 6473) 正在运行...
[root@node1 ~]$ ss -tnl | grep 21　　　　　　　　　　　　　　　　　　　　#默认监听21号端口 
LISTEN     0      32                        *:21                       *:*
[root@bitspace ~]# service vsftpd start
Starting vsftpd for vsftpd:                                [  OK  ]
[root@bitspace ~]# netstat -anltp | grep 21
tcp        0      0 0.0.0.0:21                  0.0.0.0:*                   LISTEN      4411/vsftpd         
tcp        0      0 10.88.88.22:60614           183.61.241.214:80           TIME_WAIT   -                   
tcp        0      0 10.88.88.22:40278           121.9.244.94:80             TIME_WAIT   -                   
tcp        0      0 10.88.88.22:51054           121.9.244.91:80             TIME_WAIT   -                   
[root@bitspace ~]# 
```


### 3.访问vsftpd服务器

然后把相应端口（21）打开。让其它机器可访问。

在本机或者其他主机，在其他主机上测试，需要先确认两台主机能进行网络通信ping，telnet

在linux上访问测试

```
[root@localhost ~]#lftp 172.16.55.6
lftp 172.16.55.6:~> ls              
drwxr-xr-x    2 0        0            4096 May 11  2016 pub
lftp 172.16.55.6:/> 
```
windows上访问测试

```
C:\Users\Vathe>ftp 172.16.55.6       　　　　　　　　　　#连接
Connected to 172.16.55.6.　　　　　　
(vsFTPd 2.2.2)
Always in UTF8 mode.
User (172.16.55.6:(none)): ftp　　　　　　　　　　　　　　#输入用户名
Please specify the password.
Password:　　　　　　　　　　　　　　　　　　　　　　　　　　#密码为空
Login successful.
ftp> ls　　　　　　　　　　　　　　　　　　　　　　　　　　　#显示文件　
PORT command successful. Consider using PASV.
Here comes the directory listing.
pub
Directory send OK.
ftp: 8 bytes received in 0.00Seconds 8000.00Kbytes/sec.
ftp> pwd　　　　　　　　　　　　　　　　　　　　　　　　　　　#显示当前目录
"/"
ftp> cd pub
Directory successfully changed.
```

### 4.创建本地用户admin

```
[root@node1 vsftpd]$ useradd admin
[root@node1 vsftpd]$ echo "admin" | passwd --stdin admin
更改用户 admin 的密码 。
passwd： 所有的身份验证令牌已经成功更新。
```

### 5.修改配置文件vsftpf.conf
```
[root@node1 vsftpd]$ cp vsftp.conf{,.bak}　　　　　　　　　　#备份操作
[root@node1 vsftpd]$ vim vsftpd.conf
    chroot_local_user=YES                                 #禁锢所有本地用户至家目录
    chroot_list_enable=YES                                #指定需要禁锢的本地用户
    chroot_list_file=/etc/vsftpd/chroot_list          #禁锢的用户文件

[root@node1 vsftpd]$ pwd                       
/etc/vsftpd
[root@node1 vsftpd]$ vim chroot_list               #添加admin用户
    admin
[root@node1 vsftpd]$ !ser                               #重启vsftpd服务
service vsftpd reload
关闭 vsftpd：                                              [确定]
为 vsftpd 启动 vsftpd：                                    [确定]
```

### 6.添加白名单

```
[root@node1 vsftpd]$ cd /etc/pam.d/
[root@node1 pam.d]$ vim vsftpd                             #修改pam的配置文件为白名单
    auth  required pam_listfile.so item=user sense=allow file=/etc/vsftpd/ftpusers onerr=succeed

[root@node1 vsftpd]$ cp ftpusers{,.bak}                    #备份
[root@node1 vsftpd]$ vim ftpusers                          #修改白名单列表
    # Users that are not allowed to login via ftp
    admin
    ftp
```
注：也可以使用vsftpd自身的配置文件/etc/vsftpd/user_list进行配置

### 测试登录用户

```
[root@localhost ~]#ftp 172.16.55.6                # 连接
Connected to 172.16.55.6 (172.16.55.6).
(vsFTPd 2.2.2)
Name (172.16.55.6:root): ftp　　　　　　　　　　　　　# ftp用户登录成功
Please specify the password.
Password:                                         # 不需要密码登录
Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> exit
Goodbye.

[root@localhost ~]#ftp 172.16.55.6
Connected to 172.16.55.6 (172.16.55.6).
(vsFTPd 2.2.2)
Name (172.16.55.6:root): vathe                    # 其他用户（非白名单用户）登录
Please specify the password.
Password:
Login incorrect. 　　　　　　　　　　　　　　　　　
Login failed.                                     # 登录失败
ftp>
```

### vstfpd服务常见文件目录

```
[root@node1 ~]$ rpm -ql vsftpd                          #查看程序包相关文件
/etc/logrotate.d/vsftpd　　　　　　　　　　　　　　　　　　　
/etc/pam.d/vsftpd     
/etc/rc.d/init.d/vsftpd                                 #主程序文件
/etc/vsftpd/ftpusers                                    #pam模块默认的黑名单配置文件
/etc/vsftpd/user_list                                   #黑名单或白名单配置文件
/etc/vsftpd/vsftpd.conf                                 #主配置文件
...
/usr/share/doc/vsftpd-2.2.2/EXAMPLE/INTERNET_SITE       #示例文档 
2.2.2/EXAMPLE/INTERNET_SITE/vsftpd.xinetd
/usr/share/doc/vsftpd-2.2.2/EXAMPLE/INTERNET_SITE_NOINETD
/usr/share/doc/vsftpd-2.2.2/EXAMPLE/INTERNET_SITE_NOINETD/README
/usr/share/doc/vsftpd-2.2.2/EXAMPLE/INTERNET_SITE_NOINETD/README.configuration
/usr/share/doc/vsftpd-2.2.2/EXAMPLE/INTERNET_SITE_NOINETD/vsftpd.conf
/usr/share/doc/vsftpd-2.2.2/EXAMPLE/PER_IP_CONFIG/hosts.allow  
/usr/share/doc/vsftpd-2.2.2/EXAMPLE/VIRTUAL_USERS/vsftpd.conf
/usr/share/doc/vsftpd-2.2.2/EXAMPLE/VIRTUAL_USERS_2
/usr/share/doc/vsftpd-2.2.2/EXAMPLE/VIRTUAL_USERS_2/README
/usr/share/doc/vsftpd-2.2.2/FAQ
...
/usr/share/man/man8/vsftpd.8.gz
/var/ftp                                                #匿名用户共享资源路径
/var/ftp/pub
```

## Ref

- https://www.cnblogs.com/vathe/p/6950325.html
