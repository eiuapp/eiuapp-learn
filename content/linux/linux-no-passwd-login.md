---
title: "Linux No Passwd Login"
date: 2018-07-14T17:44:08+08:00
draft: false
tags: ["linux"]
categories: ["linux"]
subtitle: "ssh禁止密码登录"
descriptions: ""
bigimg:
---

关闭SSH传统密码登陆方式

## Env

- os: linux


## Step

### 加入登录公钥

自己创建 或 从其它地方导入 `.ssh/authorized_keys` 文件
```
root@ubuntu:/home/ubuntu# mv ssh/ .ssh/
root@ubuntu:/home/ubuntu# chmod 600 .ssh/authorized_keys
```

### 修改SSH的配置文件/etc/ssh/sshd_config

```
root@ubuntu:/home/ubuntu# vi /etc/ssh/sshd_config 
root@ubuntu:/home/ubuntu# grep PasswordAuthentication -rn  /etc/ssh/sshd_config
52:#PasswordAuthentication yes
53:PasswordAuthentication no
83:# PasswordAuthentication.  Depending on your PAM configuration,
87:# PAM authentication, then enable this but set PasswordAuthentication
root@ubuntu:/home/ubuntu# 
```

注意，这个地方，一定要把 .ssh 文件夹的属性设置正确，然后重启sshd服务，否则会有如下错误

```
ubuntu@ubuntu:~$ sudo systemctl status sshd
● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2018-07-16 19:51:44 CST; 34s ago
 Main PID: 1439 (sshd)
    Tasks: 3
   Memory: 6.4M
      CPU: 58ms
   CGroup: /system.slice/ssh.service
           ├─1330 sshd: ubuntu [priv]
           ├─1331 sshd: ubuntu [net]
           └─1439 /usr/sbin/sshd -D

Jul 16 19:52:02 ubuntu sshd[1445]: error: Received disconnect from 10.88.88.64 port 49464:0:  [preauth]
Jul 16 19:52:02 ubuntu sshd[1445]: Disconnected from 10.88.88.64 port 49464 [preauth]
Jul 16 19:52:07 ubuntu sshd[1447]: Authentication refused: bad ownership or modes for directory /home/ubuntu/.ssh
Jul 16 19:52:10 ubuntu sshd[1447]: Authentication refused: bad ownership or modes for directory /home/ubuntu/.ssh
Jul 16 19:52:11 ubuntu sshd[1447]: Authentication refused: bad ownership or modes for directory /home/ubuntu/.ssh
Jul 16 19:52:12 ubuntu sshd[1447]: error: maximum authentication attempts exceeded for ubuntu from 10.88.88.64 port 49481 ssh2 [preauth]
Jul 16 19:52:12 ubuntu sshd[1447]: Disconnecting: Too many authentication failures [preauth]
ubuntu@ubuntu:~$
```

下面2步不能少

```
ubuntu@ubuntu:~$ chown -R ubuntu:ubuntu .ssh/
ubuntu@ubuntu:~$ chmod 700 .ssh/
ubuntu@ubuntu:~$
```

### 重启sshd服务

```
ubuntu@ubuntu:~$ sudo systemctl restart sshd
ubuntu@ubuntu:~$ sudo systemctl status sshd
```

## Ref

- http://www.cnblogs.com/tl542475736/p/7814097.html
- http://www.cnblogs.com/tintin1926/archive/2012/07/23/2604281.html
- https://superuser.com/questions/215504/permissions-on-private-key-in-ssh-folder