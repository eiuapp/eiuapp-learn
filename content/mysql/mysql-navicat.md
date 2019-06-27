---
title: "Mysql Navicat"
date: 2018-03-02T12:46:45+08:00
draft: false
tags: ["mysql", "navicat"]
categories: ["mysql"]
subtitle: ""
descriptions: ""
bigimg:
---

mysql系列之navicat

## reference

http://blog.csdn.net/lwei_998/article/details/45560483

http://blog.csdn.net/moneyshi/article/details/50906650

http://mt.sohu.com/20160324/n441844650.shtml

## 导出连接设置

Navicat 版本 8
1.选择文件 -> 导出登录数据文件。导出的文件（.reg）包含你的全部连接设置。
2.备份已导出的文件（.reg）。
3.在现有的计算机解除安装 Navicat。
4.在新的计算机重新安装 Navicat。
5.在新的计算机运行已导出的文件（.reg）。

Navicat 版本 9 或以上
1.在 Navicat，选择文件 -> 导出连接。导出的文件（.ncx）包含你的全部连接设置。
2.备份已导出的文件（.ncx）。
3.在现有的计算机解除安装 Navicat。**
4.在新的计算机重新安装 Navicat。
5.在新的计算机打开 Navicat 和选择文件 -> 导入连接。


** 如果你使用版本 11 或以上，请在解除安装 Navicat 前取消激活注册码。

 当创建一个新的连接，Navicat 将创建一个子文件夾（名为各数据库的名）在设置保存路径內。所有备份（.psc、.psb）、报表（.rtm）、查询（.sql）、导入/导出设置文件等都是保存在该子文件夾。要查找路径，你可以右击连接，然后选择连接属性 -> 高级 -> 设置保存路径/设置位置。


 此外，全部已保存的设置文件（批处理作业设置文件）会保存在 profiles 文件夾。要查找路径，选择工具 -> 选项 -> 其他 -> 设置文件保存路径/设置文件位置。

##  异常收集之：navicatdesignquery.sql.bak 系统找不到指定路径

今天使用Navicat ，其他功能都正常，但是新建查询的时候，出现一个很奇葩的问题

`C:\Program Files (x86)\PremiumSoft\Navicat for MySQL8.1/_NAVICAT_DESIGNQUERY.sql.bak`  系统找不到指定路径

找了半天找不到解决办法，下载navicat 11都没用， 更改版本也没用。

最后发现，navicat 的每个连接，有个连接属性

具体操作：
在连接---属性---高级。修改一下路径，改成你现在安装的navicat目录就好了

## 如何迁移 Navicat 到新的计算机

迁移Navicat到新的计算机的步骤：

　　1. 选择文件->导出连接。导出的文件（.ncx）包含了全部连接设置内容。
　　2. 备份已导出的文件（.ncx）。
　　3. 在Navicat，选择帮助->注册，并点击“取消激活”来在线取消激活Navicat注册码。
　　4. 在现有的计算机解除安装Navicat。
　　5. 在新的计算机重新安装Navicat。
　　6. 在新的计算机中，打开Navicat，选择文件->导入连接。

当创建一个新的连接，Navicat将在设置位置创建一个子文件夹。大多数文件都保存在该子文件夹，右击选择属性->打开文件位置可查找路径。

此外，全部已保存的设置文件会保存在Profiles文件夹，选择工具->选项->其他->文件位置，即可查找存储路径。
