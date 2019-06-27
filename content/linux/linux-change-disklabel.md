---
title: "Linux Change Disklabel"
date: 2018-02-28T23:01:56+08:00
draft: false
tags: ["linux", "parted"]
categories: ["linux"]
subtitle: ""
descriptions: ""
bigimg:
---

#### [parted 來手動改變硬碟為 GPT(或msdos等)](http://blog.csdn.net/myweishanli/article/details/42953481)

```
[anaconda root@localhost /]# parted /dev/sda
GUN Parted 2.1
Using /dev/sda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) mklabel
New disk label type?gpt
Warning: The existing disk label on /dev/sda will be destroyed and all data on this disk will be lost. Do you want to continue?
Yes/No?Yes
(parted) q
Information: You may need to update /etc/fstab.
```