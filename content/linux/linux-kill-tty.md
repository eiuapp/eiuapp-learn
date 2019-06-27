---
title: "Linux Kill Tty"
date: 2018-06-19T17:41:03+08:00
draft: false
tags: ["linux", "tty"]
categories: ["linux"]
subtitle: "linux 如何杀掉 tty终端"
descriptions: ""
bigimg:
---

linux 如何杀掉 tty终端


## Step

### 1、用w -s命令可以得到终端名

```
root@bitzone:~# w
 17:37:05 up 164 days,  1:13,  2 users,  load average: 1.47, 0.83, 0.66
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    121.34.147.224   17:36    0.00s  0.02s  0.00s w
root     pts/2    121.34.147.224   11:58    5:37m  0.04s  0.04s -bash
root@bitzone:~# w -s
 17:38:38 up 164 days,  1:15,  2 users,  load average: 0.65, 0.69, 0.62
USER     TTY      FROM              IDLE WHAT
root     pts/0    121.34.147.224    2.00s w -s
root     pts/2    121.34.147.224    5:39m -bash
root@bitzone:~# 
```

### 2、用ps -t 命令可以得到终端的进程号

```
root@bitzone:~# ps -t /dev/pts/2
  PID TTY          TIME CMD
14188 pts/2    00:00:00 bash
root@bitzone:~# 
```
### 3、用kill -9命令

可以将进程杀掉，以关闭终端。
s前提：kill命令的执行者必须是超级用户或对tty1的进程有操作权限，否则，命令会报错：Operation not permitted，如：

```
root@bitzone:~# ps -t /dev/pts/2
  PID TTY          TIME CMD
14188 pts/2    00:00:00 bash
root@bitzone:~# kill -9 14188
root@bitzone:~# w
 17:39:06 up 164 days,  1:15,  1 user,  load average: 0.54, 0.66, 0.61
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    121.34.147.224   17:36    2.00s  0.04s  0.00s w
root@bitzone:~# 
```

## 命令汇总


```
root@bitzone:~# w
 17:37:05 up 164 days,  1:13,  2 users,  load average: 1.47, 0.83, 0.66
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    121.34.147.224   17:36    0.00s  0.02s  0.00s w
root     pts/2    121.34.147.224   11:58    5:37m  0.04s  0.04s -bash
root@bitzone:~# w -s
 17:38:38 up 164 days,  1:15,  2 users,  load average: 0.65, 0.69, 0.62
USER     TTY      FROM              IDLE WHAT
root     pts/0    121.34.147.224    2.00s w -s
root     pts/2    121.34.147.224    5:39m -bash
root@bitzone:~# ps -t /dev/pts/2
  PID TTY          TIME CMD
14188 pts/2    00:00:00 bash
root@bitzone:~# kill -9 14188
root@bitzone:~# w
 17:39:06 up 164 days,  1:15,  1 user,  load average: 0.54, 0.66, 0.61
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    121.34.147.224   17:36    2.00s  0.04s  0.00s w
root@bitzone:~# 
```

## Ref

- https://blog.csdn.net/r13929847477/article/details/77979146