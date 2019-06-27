---
title: "Mount Ntfs Base Centos"
date: 2018-06-06T16:57:02+08:00
draft: false
tags: ["centos","mount"]
categories: ["mount"]
subtitle: "centos挂载ntfs硬盘"
descriptions: ""
bigimg:
---

centos挂载ntfs硬盘

## Env

- os: centos6.9

当前状态

```
[root@bitspace ~]# lsblk
NAME                           MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                              8:0    0 931.5G  0 disk 
├─sda1                           8:1    0   220G  0 part 
├─sda2                           8:2    0   238G  0 part 
├─sda3                           8:3    0   237G  0 part 
└─sda4                           8:4    0 236.5G  0 part 
sdb                              8:16   0 111.8G  0 disk 
├─sdb1                           8:17   0   500M  0 part /boot
└─sdb2                           8:18   0 111.3G  0 part 
  ├─vg_bitspace-lv_root (dm-0) 253:0    0    50G  0 lvm  /
  ├─vg_bitspace-lv_swap (dm-1) 253:1    0   7.7G  0 lvm  [SWAP]
  └─vg_bitspace-lv_home (dm-2) 253:2    0  53.6G  0 lvm  /home
[root@bitspace ~]# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/vg_bitspace-lv_root
                       50G  3.1G   44G   7% /
tmpfs                 3.8G     0  3.8G   0% /dev/shm
/dev/sdb1             477M   41M  411M   9% /boot
/dev/mapper/vg_bitspace-lv_home
                       53G   53M   50G   1% /home
[root@bitspace ~]# fdisk -l

WARNING: GPT (GUID Partition Table) detected on '/dev/sda'! The util fdisk doesn't support GPT. Use GNU Parted.


Disk /dev/sda: 1000.2 GB, 1000204886016 bytes
256 heads, 63 sectors/track, 121126 cylinders
Units = cylinders of 16128 * 512 = 8257536 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disk identifier: 0x0f544158

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1               1      266306  2147483647+  ee  GPT
Partition 1 does not start on physical sector boundary.

Disk /dev/sdb: 120.0 GB, 120034123776 bytes
255 heads, 63 sectors/track, 14593 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0bd932d6

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1   *           1          64      512000   83  Linux
Partition 1 does not end on cylinder boundary.
/dev/sdb2              64       14594   116707328   8e  Linux LVM

Disk /dev/mapper/vg_bitspace-lv_root: 53.7 GB, 53687091200 bytes
255 heads, 63 sectors/track, 6527 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000


Disk /dev/mapper/vg_bitspace-lv_swap: 8271 MB, 8271167488 bytes
255 heads, 63 sectors/track, 1005 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000


Disk /dev/mapper/vg_bitspace-lv_home: 57.5 GB, 57545850880 bytes
255 heads, 63 sectors/track, 6996 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

[root@bitspace ~]# 
```

## Step

```
[root@bitspace ~]# mount /dev/sda1 /mnt/sda1
mount: unknown filesystem type 'ntfs'
[root@bitspace ~]# mount -t ntfs /dev/sda1 /mnt/sda1
mount: unknown filesystem type 'ntfs'
```

没有mount.ntfs类型啊

注意：mount是一个root命令，归属于`/sbin/mount*`而不是`/bin/mount*`
```
[root@bitspace ~]# which mount
/bin/mount
[root@bitspace ~]# ls /sbin/moun*
/sbin/mount.cifs  /sbin/mount.nfs  /sbin/mount.nfs4  /sbin/mount.tmpfs
[root@bitspace ~]#
```
说明没有相关的mount类型呀


```
[root@bitspace ntfs-3g_ntfsprogs-2017.3.23]# mount -t ntfs /dev/sda1 /mnt/sda1
mount: unknown filesystem type 'ntfs'
```

没成功

解决办法：
 
通过使用 ntfs-3g 来解决。 
打开ntfs-3g的下载点http://www.tuxera.com/community/ntfs-3g-download/ ，将最新稳定(当前最新版本为[ntfs-3g_ntfsprogs-2017.3.23](https://tuxera.com/opensource/ntfs-3g_ntfsprogs-2017.3.23.tgz)）下载到CentOS，执行以下命令安装： 

### 编译安装 ntfs-3g

```
# tar zxvf ntfs-3g_ntfsprogs-2017.3.23.tgz
# cd ntfs-3g_ntfsprogs-2017.3.23
#./configure
# make
# make install
```

在网上找了，方法一样安上 还是不能挂载，最后在官方站 找到方法了，如下：
```
mount -t ntfs-3g /dev/sda5 /mnt/windows 
```
这样就可以挂载了, 试一下

```
[root@bitspace ntfs-3g_ntfsprogs-2017.3.23]# ls /sbin/moun*
/sbin/mount.cifs  /sbin/mount.lowntfs-3g  /sbin/mount.nfs  /sbin/mount.nfs4  /sbin/mount.ntfs-3g  /sbin/mount.tmpfs
[root@bitspace ntfs-3g_ntfsprogs-2017.3.23]# mount -t ntfs-3g /dev/sda1 /mnt/sda1
The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
Falling back to read-only mount because the NTFS partition is in an
unsafe state. Please resume and shutdown Windows fully (no hibernation
or fast restarting.)
[root@bitspace ntfs-3g_ntfsprogs-2017.3.23]#
```
看到这条信息，不要紧张，实际上就是已经挂载成功了。哈哈，但是这个信息，给人的感觉是没有成功。

```
[root@bitspace ~]# ls /mnt/sda1/
$RECYCLE.BIN  System Volume Information
[root@bitspace ~]# ls /mnt/sda1/System\ Volume\ Information/
IndexerVolumeGuid  tracking.log
```

其它几个类似
```
[root@bitspace ~]# ls /mnt/sda2/
$RECYCLE.BIN  System Volume Information
[root@bitspace ~]# ls /mnt/sda3/
$RECYCLE.BIN  System Volume Information
[root@bitspace ~]# ls /mnt/sda4/
$RECYCLE.BIN  System Volume Information
```

查看一下所有硬盘

```
[root@bitspace ~]# df -h
Filesystem            Size  Used Avail Use% Mounted on
/dev/mapper/vg_bitspace-lv_root
                       50G  3.2G   44G   7% /
tmpfs                 3.8G     0  3.8G   0% /dev/shm
/dev/sdb1             477M   41M  411M   9% /boot
/dev/mapper/vg_bitspace-lv_home
                       53G   53M   50G   1% /home
/dev/sda1             221G  120M  220G   1% /mnt/sda1
/dev/sda2             239G  121M  238G   1% /mnt/sda2
/dev/sda3             238G  121M  237G   1% /mnt/sda3
/dev/sda4             237G  121M  237G   1% /mnt/sda4
[root@bitspace ~]# 
```

## Ref

- http://blog.51cto.com/luyafei/496788
- https://www.tuxera.com/community/open-source-ntfs-3g/