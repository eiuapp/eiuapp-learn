---
title: "Gitlab Install"
date: 2018-03-02T11:40:25+08:00
draft: false
tags: ["gitlab", "install"]
categories: ["gitlab"]
subtitle: ""
descriptions: ""
bigimg:
gitment: true
---

git系列之gitlab安装

## env

阿里云 
- IP: 120.25.204.216
- OS: centos7


## step

```
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
```
注意这个写法

```
sudo EXTERNAL_URL="http://gitlab.example.com" yum install -y gitlab-ee
```
修改成 下面这种 方式, 但是, 这样下载会比较慢了
```
sudo EXTERNAL_URL="http://120.25.204.216:7890" yum install -y gitlab-ee
```

我们用 [清华镜像](https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/) 吧

```
[root@jlch_web_001 ~]# wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-10.6.2-ce.0.el7.x86_64.rpm
--2018-04-12 15:11:25--  https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-10.6.2-ce.0.el7.x86_64.rpm
Resolving mirrors.tuna.tsinghua.edu.cn (mirrors.tuna.tsinghua.edu.cn)... 101.6.8.193, 2402:f000:1:408:8100::1
Connecting to mirrors.tuna.tsinghua.edu.cn (mirrors.tuna.tsinghua.edu.cn)|101.6.8.193|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 417757949 (398M) [application/x-redhat-package-manager]
Saving to: ‘gitlab-ce-10.6.2-ce.0.el7.x86_64.rpm’

100%[==================================================================================================================================================================>] 417,757,949 19.1MB/s   in 21s

2018-04-12 15:11:46 (18.6 MB/s) - ‘gitlab-ce-10.6.2-ce.0.el7.x86_64.rpm’ saved [417757949/417757949]

[root@jlch_web_001 ~]# rpm -ivh gitlab-ce-10.6.2-ce.0.el7.x86_64.rpm
warning: gitlab-ce-10.6.2-ce.0.el7.x86_64.rpm: Header V4 RSA/SHA1 Signature, key ID f27eab47: NOKEY
Preparing...                          ################################# [100%]
Updating / installing...
   1:gitlab-ce-10.6.2-ce.0.el7        ################################# [100%]
It looks like GitLab has not been configured yet; skipping the upgrade script.

       *.                  *.
      ***                 ***
     *****               *****
    .******             *******
    ********            ********
   ,,,,,,,,,***********,,,,,,,,,
  ,,,,,,,,,,,*********,,,,,,,,,,,
  .,,,,,,,,,,,*******,,,,,,,,,,,,
      ,,,,,,,,,*****,,,,,,,,,.
         ,,,,,,,****,,,,,,
            .,,,***,,,,
                ,*,.



     _______ __  __          __
    / ____(_) /_/ /   ____ _/ /_
   / / __/ / __/ /   / __ `/ __ \
  / /_/ / / /_/ /___/ /_/ / /_/ /
  \____/_/\__/_____/\__,_/_.___/


Thank you for installing GitLab!
GitLab was unable to detect a valid hostname for your instance.
Please configure a URL for your GitLab instance by setting `external_url`
configuration in /etc/gitlab/gitlab.rb file.
Then, you can start your GitLab instance by running the following command:
  sudo gitlab-ctl reconfigure

For a comprehensive list of configuration options please see the Omnibus GitLab readme
https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md

[root@jlch_web_001 ~]#
```


然后修改IP端口吧

```
vim  /etc/gitlab/gitlab.rb
### 修改内容为: external_url 'http://120.25.204.216:7890'
```
然后 
```
sudo gitlab-ctl reconfigure
```
等待
```
gitlab-ctl restart
```
#### FAQ

```
sudo systemctl start postfix
```
出错了

启动postfix出错，查看centos中的postfix日志
```
more  /var/log/maillog

postfix: fatal: parameter inet_interfaces: no local interface found for ::1
```
修改postfix的配置文件

```
vi  /etc/postfix/main.cf
```


发现配置为：
```
inet_interfaces = localhost

inet_protocols = all
```
改成：
```
inet_interfaces = all

inet_protocols = all
```

重新启动
```
sudo systemctl start postfix
[root@jlch_web_001 tom]# sudo systemctl status postfix
● postfix.service - Postfix Mail Transport Agent
   Loaded: loaded (/usr/lib/systemd/system/postfix.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2018-04-12 14:51:04 CST; 4s ago
  Process: 13058 ExecStart=/usr/sbin/postfix start (code=exited, status=0/SUCCESS)
  Process: 13054 ExecStartPre=/usr/libexec/postfix/chroot-update (code=exited, status=0/SUCCESS)
  Process: 13049 ExecStartPre=/usr/libexec/postfix/aliasesdb (code=exited, status=0/SUCCESS)
 Main PID: 13130 (master)
   Memory: 3.6M
   CGroup: /system.slice/postfix.service
           ├─13130 /usr/libexec/postfix/master -w
           ├─13131 pickup -l -t unix -u
           └─13132 qmgr -l -t unix -u

Apr 12 14:51:03 jlch_web_001 systemd[1]: Starting Postfix Mail Transport Agent...
Apr 12 14:51:03 jlch_web_001 postfix/postfix-script[13128]: starting the Postfix mail system
Apr 12 14:51:04 jlch_web_001 postfix/master[13130]: daemon started -- version 2.10.1, configuration /etc/postfix
Apr 12 14:51:04 jlch_web_001 systemd[1]: Started Postfix Mail Transport Agent.
[root@jlch_web_001 tom]#
```


## ref

https://gitlab.com/gitlab-org/gitlab-ce/tree/master
- [https://about.gitlab.com/installation/#centos-7](https://about.gitlab.com/installation/#centos-7)
- [postfix报错postfix: fatal: parameter inet_interfaces: no local interface found for ::1](https://blog.csdn.net/xiangshanqishi/article/details/23439397)
- [centos7安装部署gitlab服务器](https://www.cnblogs.com/wenwei-blog/p/5861450.html)