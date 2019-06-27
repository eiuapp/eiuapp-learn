---
title: "Mount"
date: 2018-02-22T10:20:04+08:00
draft: false
tags: ["mount"]
categories: ["mount"]
subtitle: ""
descriptions: ""
bigimg:
---

#### 通过 shell

```
root@ud-31-193:~# mkdir /mnt/31.10.share
root@ud-31-193:~# mount -t cifs -o username=share,password="" //192.168.31.10/share /mnt/31.10.share/
root@ud-31-193:~#
```

#### 通过 auto.misc

```
[jlch@check conf.d]$ sudo cat /etc/auto.misc
[sudo] password for jlch:
#
# This is an automounter map and it has the following format
# key [ -mount-options-separated-by-comma ] location
# Details may be found in the autofs(5) manpage

#cd		-fstype=iso9660,ro,nosuid,nodev	:/dev/cdrom
#dbf             -fstype=cifs,rw,username=administrator,password=jlch@2016  ://10.10.14.11/hq
share         -fstype=cifs,rw,username=share,password=  ://192.168.31.10/share
31245share         -fstype=cifs,rw,username=share,password=Jlch1234  ://192.168.31.245/share
319share         -fstype=cifs,rw,username=user01,password=User01  ://192.168.31.9/DataVol2/

dbf             -fstype=cifs,rw,username=administrator,password=jlch333  ://10.10.12.202/hq
shdef           -fstype=cifs,rw,username=administrator,password=jlch333  ://10.10.12.201/dbf

sse_mstp         -fstype=cifs,rw,username=ituser,password=!Tuser0708  ://172.39.187.210/sse_mstp_m
szsefiles        -fstype=cifs,rw,username=ituser,password=!Tuser0708  ://10.198.7.226/SZSEFiles

sse_mstp0         -fstype=cifs,rw,username=ituser,password=!Tuser0708  ://172.39.187.210/data

128share         -fstype=cifs,rw,username=ituser,password=SVMSmZ77a7  ://192.168.31.128/Z
```

