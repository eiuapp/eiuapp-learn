---
title: "Linux Add Format Mount Harddisk"
date: 2018-06-28T14:16:58+08:00
draft: false
tags: ["linux", "ubuntu"]
categories: ["linux"]
subtitle: "替 Linux 新增硬碟（磁碟分割、格式化與掛載）"
descriptions: ""
bigimg:
---

替 Linux 新增硬碟（磁碟分割、格式化與掛載）

最近要替我的 Linux Server 增加一顆硬碟，一般若是在安裝 Linux 時就將硬碟裝上去的話，就可以直接在安裝時設定好硬碟的格式化與掛載，但若是後來要加掛新的硬碟，就要自己動手設定了。

這裡示範在 Ubuntu Linux 下面新增加硬碟的做法，首先當然是買一顆新硬碟囉。

## Env

- os: ubuntu16
- 新硬盘: sdb

## Step

### 查看

```
root@ubuntu:~# fdisk -l
Disk /dev/sda: 111.8 GiB, 120034123776 bytes, 234441648 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 846C81DB-5DB9-4853-94E9-CC1192592FC9

Device       Start       End   Sectors   Size Type
/dev/sda1     2048   1050623   1048576   512M EFI System
/dev/sda2  1050624   2050047    999424   488M Linux filesystem
/dev/sda3  2050048 234440703 232390656 110.8G Linux LVM


Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x00c57ca5

Device     Boot      Start        End    Sectors   Size Id Type
/dev/sdb1             2048 1953523711 1953521664 931.5G  f W95 Ext'd (LBA)
/dev/sdb5             4096  629151743  629147648   300G  7 HPFS/NTFS/exFAT
/dev/sdb6        629153792 1291855871  662702080   316G  7 HPFS/NTFS/exFAT
/dev/sdb7       1291857920 1953523711  661665792 315.5G  7 HPFS/NTFS/exFAT


Disk /dev/mapper/ubuntu--vg-root: 109.9 GiB, 117952217088 bytes, 230375424 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/mapper/ubuntu--vg-swap_1: 980 MiB, 1027604480 bytes, 2007040 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
root@ubuntu:~# fdisk -l /dev/sdb
Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x00c57ca5

Device     Boot      Start        End    Sectors   Size Id Type
/dev/sdb1             2048 1953523711 1953521664 931.5G  f W95 Ext'd (LBA)
/dev/sdb5             4096  629151743  629147648   300G  7 HPFS/NTFS/exFAT
/dev/sdb6        629153792 1291855871  662702080   316G  7 HPFS/NTFS/exFAT
/dev/sdb7       1291857920 1953523711  661665792 315.5G  7 HPFS/NTFS/exFAT
root@ubuntu:~# 
```

可以看到，这不是全新的，应该是之前有人用过的硬盘，被分过区。

### 删除原来的分区

```
root@ubuntu:~# fdisk  /dev/sdb

Welcome to fdisk (util-linux 2.27.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): p
Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x00c57ca5

Device     Boot      Start        End    Sectors   Size Id Type
/dev/sdb1             2048 1953523711 1953521664 931.5G  f W95 Ext'd (LBA)
/dev/sdb5             4096  629151743  629147648   300G  7 HPFS/NTFS/exFAT
/dev/sdb6        629153792 1291855871  662702080   316G  7 HPFS/NTFS/exFAT
/dev/sdb7       1291857920 1953523711  661665792 315.5G  7 HPFS/NTFS/exFAT

Command (m for help): d
Partition number (1,5-7, default 7): 

Partition 7 has been deleted.

Command (m for help): d
Partition number (1,5,6, default 6): 

Partition 6 has been deleted.

Command (m for help): d
Partition number (1,5, default 5): 

Partition 5 has been deleted.

Command (m for help): w
The partition table has been altered.
qCalling ioctl() to re-read partition table.
Syncing disks.

root@ubuntu:~# fdisk -l /dev/sdb
Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x00c57ca5

Device     Boot Start        End    Sectors   Size Id Type
/dev/sdb1        2048 1953523711 1953521664 931.5G  f W95 Ext'd (LBA)
root@ubuntu:~# 
```

好了，已删除，把它当成一块新硬盘吧。


### 新建立一个逻辑分区吧

```
root@ubuntu:~# mkfs -t ext4 /dev/sdb1 
mke2fs 1.42.13 (17-May-2015)
Found a dos partition table in /dev/sdb1
Proceed anyway? (y,n) y
mkfs.ext4: inode_size (128) * inodes_count (0) too big for a
	filesystem with 0 blocks, specify higher inode_ratio (-i)
	or lower inode count (-N).

root@ubuntu:~# which mkfs.ext4
/sbin/mkfs.ext4
root@ubuntu:~# mkfs.ext4 /dev/sdb1
mke2fs 1.42.13 (17-May-2015)
Found a dos partition table in /dev/sdb1
Proceed anyway? (y,n) y
mkfs.ext4: inode_size (128) * inodes_count (0) too big for a
	filesystem with 0 blocks, specify higher inode_ratio (-i)
	or lower inode count (-N).

root@ubuntu:~# 
```

好吧，因为不是逻辑分区，所以，挂载失败了。
新建立一个逻辑分区吧

```
root@ubuntu:~# fdisk /dev/sdb

Welcome to fdisk (util-linux 2.27.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): n
All space for primary partitions is in use.
Adding logical partition 5
First sector (4096-1953523711, default 4096): 
Last sector, +sectors or +size{K,M,G,T,P} (4096-1953523711, default 1953523711): 

Created a new partition 5 of type 'Linux' and of size 931.5 GiB.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

root@ubuntu:~#
root@ubuntu:~# fdisk -l /dev/sdb
Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: dos
Disk identifier: 0x00c57ca5

Device     Boot Start        End    Sectors   Size Id Type
/dev/sdb1        2048 1953523711 1953521664 931.5G  f W95 Ext'd (LBA)
/dev/sdb5        4096 1953523711 1953519616 931.5G 83 Linux
root@ubuntu:~# 
```

### 查看一下块信息

```
root@ubuntu:~# blkid 
/dev/sda1: UUID="BED7-5A24" TYPE="vfat" PARTUUID="cfc28357-02bf-4471-b8df-a6a6583fdbd3"
/dev/sda2: UUID="2729da44-8e4e-4f0c-ae39-97e2b0feb31e" TYPE="ext2" PARTUUID="8f7353d4-83d3-4196-80e9-d0e01fb7ed7f"
/dev/sda3: UUID="7hQ9xw-bLp9-3XSb-ngFg-hCBF-O2vO-djLqei" TYPE="LVM2_member" PARTUUID="59163f66-90e5-466d-9d48-b40fe2ee367e"
/dev/mapper/ubuntu--vg-root: UUID="32e5d6dd-a435-48f7-9604-ab426300f93d" TYPE="ext4"
/dev/mapper/ubuntu--vg-swap_1: UUID="a1551b61-32df-4853-ba8e-55dea76aa580" TYPE="swap"
/dev/sdb5: UUID="114dc4e0-32e1-4e0c-bcbd-12812e67b5d6" TYPE="ext4" PARTUUID="00c57ca5-05"
root@ubuntu:~# cat /etc/fstab 
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
/dev/mapper/ubuntu--vg-root /               ext4    errors=remount-ro 0       1
# /boot was on /dev/sda2 during installation
UUID=2729da44-8e4e-4f0c-ae39-97e2b0feb31e /boot           ext2    defaults        0       2
# /boot/efi was on /dev/sda1 during installation
UUID=BED7-5A24  /boot/efi       vfat    umask=0077      0       1
/dev/mapper/ubuntu--vg-swap_1 none            swap    sw              0       0
root@ubuntu:~# 
```

### mount

注意，mount的写法，block信息写前，mount地址写后。

```
root@ubuntu:~# mount -t ext4 /mnt/sdb/ /dev/sdb5
mount:  /mnt/sdb is not a block device
root@ubuntu:~# mount -t ext4 /dev/sdb5 /mnt/sdb
root@ubuntu:~# ls /mnt/sdb/
lost+found
root@ubuntu:~# 
```

### 确认

```
root@ubuntu:~# blkid
/dev/sda1: UUID="BED7-5A24" TYPE="vfat" PARTUUID="cfc28357-02bf-4471-b8df-a6a6583fdbd3"
/dev/sda2: UUID="2729da44-8e4e-4f0c-ae39-97e2b0feb31e" TYPE="ext2" PARTUUID="8f7353d4-83d3-4196-80e9-d0e01fb7ed7f"
/dev/sda3: UUID="7hQ9xw-bLp9-3XSb-ngFg-hCBF-O2vO-djLqei" TYPE="LVM2_member" PARTUUID="59163f66-90e5-466d-9d48-b40fe2ee367e"
/dev/mapper/ubuntu--vg-root: UUID="32e5d6dd-a435-48f7-9604-ab426300f93d" TYPE="ext4"
/dev/mapper/ubuntu--vg-swap_1: UUID="a1551b61-32df-4853-ba8e-55dea76aa580" TYPE="swap"
/dev/sdb5: UUID="114dc4e0-32e1-4e0c-bcbd-12812e67b5d6" TYPE="ext4" PARTUUID="00c57ca5-05"
root@ubuntu:~# df -h
Filesystem                   Size  Used Avail Use% Mounted on
udev                         3.9G     0  3.9G   0% /dev
tmpfs                        788M   18M  771M   3% /run
/dev/mapper/ubuntu--vg-root  109G  1.8G  101G   2% /
tmpfs                        3.9G     0  3.9G   0% /dev/shm
tmpfs                        5.0M     0  5.0M   0% /run/lock
tmpfs                        3.9G     0  3.9G   0% /sys/fs/cgroup
/dev/sda2                    473M   66M  384M  15% /boot
/dev/sda1                    511M  3.4M  508M   1% /boot/efi
tmpfs                        788M     0  788M   0% /run/user/1000
/dev/sdb5                    917G   72M  871G   1% /mnt/sdb
root@ubuntu:~# 
```

OK了。
新硬盘已经可以使用了。

## Ref

- https://blog.gtwang.org/linux/linux-add-format-mount-harddisk/