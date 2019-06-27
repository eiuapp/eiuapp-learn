---
title: "Change System Datetime Base Ubuntu"
date: 2018-07-11T10:40:10+08:00
draft: false
tags: ["ubuntu"]
categories: ["ubuntu"]
subtitle: "ubuntu 修改系统时间"
descriptions: ""
bigimg:
---

## Env

- os: ubuntu16


## Step

### 原理

需要取消自动从互联网同步时间才可以的

```
timedatectl set-ntp 0
```
上面的命令可以关闭自动同步，然后你再设置就好了

如果又要打开可以运行

```
timedatectl set-ntp 1
```

### 示例

```
ubuntu@ip-172-31-46-220:~$ date
Wed Jul 11 02:08:35 UTC 2018
ubuntu@ip-172-31-46-220:~$ sudo su
root@ip-172-31-46-220:/home/ubuntu# timedatectl set-ntp 0
root@ip-172-31-46-220:/home/ubuntu# date -s "2018-07-12 02:08"
Thu Jul 12 02:08:00 UTC 2018
root@ip-172-31-46-220:/home/ubuntu# hwclock --systohc
root@ip-172-31-46-220:/home/ubuntu# date
Thu Jul 12 02:08:06 UTC 2018
root@ip-172-31-46-220:/home/ubuntu# date -s "2018-07-12 02:10"
Thu Jul 12 02:10:00 UTC 2018
root@ip-172-31-46-220:/home/ubuntu# 
```

修改成功了

### 还原

```
root@ip-172-31-46-220:/home/ubuntu# sudo timedatectl set-ntp 1
root@ip-172-31-46-220:/home/ubuntu# date
Wed Jul 11 02:22:39 UTC 2018
root@ip-172-31-46-220:/home/ubuntu# 
```

## Ref

- https://segmentfault.com/q/1010000010279964