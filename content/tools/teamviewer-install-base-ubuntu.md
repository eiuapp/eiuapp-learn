---
title: "Teamviewer Install Base Ubuntu"
date: 2018-06-21T16:48:05+08:00
draft: false
tags: ["ubuntu", "teamviewer"]
categories: ["teamviewer"]
subtitle: "Ubuntu16.04 安装 Teamviewer"
descriptions: ""
bigimg:
---

Ubuntu16.04 安装 Teamviewer

有时需要远程控制ubuntu系统的电脑，Teamviewer在linux下也可以进行安装，而且Teamviewer远程控制的流畅性一直不错，就选择安装Teamviewer。

## Env

- os: ubuntu16.04
- teamviewer: 13.1

## Step


下面给出具体的安装步骤：


首先到 https://www.teamviewer.com/zhcn/download/linux/ 下载相应linux版本的Teamviewer，版主选择的是ubuntu版本，下载完成之后，在你的下载路径中会有软件安装包teamviewer_12.0.85001_i386.deb。

安装依赖包，ternimal终端进入到下载路径中，执行命令：(博主是64位系统没有执行这个命令也成功，假如是32位的系统则需要执行)

```
sudo apt-get install libjpeg62:i386 libxinerama1:i386 libxrandr2:i386 libxtst6:i386 ca-certificates -y
```

安装deb软件包，执行命令：

```
sudo dpkg -i teamviewer_12.0.76279_i386.deb
```

安装成功之后在dash输入Teamviewer就可以打开了。

注意：在执行第三步安装deb包的时候，可能会遇到下面的问题：

```
wanglaotou@wanglaotou95:~/softwares/software-package$ sudo dpkg -i teamviewer_12.0.85001_i386.deb 
(正在读取数据库 ... 系统当前共安装有 215790 个文件和目录。)
正准备解包 teamviewer_12.0.85001_i386.deb  ...
正在将 teamviewer:i386 (12.0.85001) 解包到 (12.0.85001) 上 ...
dpkg: 依赖关系问题使得 teamviewer:i386 的配置工作不能继续：
 teamviewer:i386 依赖于 libasound2.
 teamviewer:i386 依赖于 libdbus-1-3.
 teamviewer:i386 依赖于 libexpat1.
 teamviewer:i386 依赖于 libfontconfig1.
 teamviewer:i386 依赖于 libfreetype6.
 teamviewer:i386 依赖于 libsm6.
 teamviewer:i386 依赖于 libxdamage1.
 teamviewer:i386 依赖于 libxfixes3.
 teamviewer:i386 依赖于 zlib1g.

dpkg: 处理软件包 teamviewer:i386 (--install)时出错：
 依赖关系问题 - 仍未被配置
正在处理用于 gnome-menus (3.13.3-6ubuntu3.1) 的触发器 ...
正在处理用于 desktop-file-utils (0.22-1ubuntu5.1) 的触发器 ...
正在处理用于 bamfdaemon (0.5.3~bzr0+16.04.20160824-0ubuntu1) 的触发器 ...
Rebuilding /usr/share/applications/bamf-2.index...
正在处理用于 mime-support (3.59ubuntu1) 的触发器 ...
正在处理用于 hicolor-icon-theme (0.15-0ubuntu1) 的触发器 ...
在处理时有错误发生：
 teamviewer:i386
```

下面这个命令是修复依赖关系（depends）的命令，就是假如你的系统上有某个package不满足依赖条件，这个命令就会自动修复，安装那个package依赖的package.

这个时候需要执行命令：
```
sudo apt-get install -f
```

选择 ‘Y’，回车等待修复结束后继续执行第三步。

我的实操

```
tom@ud-b:~/Downloads$ sudo dpkg -i teamviewer_13.1.8286_amd64.deb 
[sudo] password for tom: 
Selecting previously unselected package teamviewer.
(Reading database ... 276613 files and directories currently installed.)
Preparing to unpack teamviewer_13.1.8286_amd64.deb ...
Unpacking teamviewer (13.1.8286) ...
dpkg: dependency problems prevent configuration of teamviewer:
 teamviewer depends on libqt5x11extras5 (>= 5.2); however:
  Package libqt5x11extras5 is not installed.
 teamviewer depends on qtdeclarative5-controls-plugin (>= 5.2) | qml-module-qtquick-controls (>= 5.2); however:
  Package qtdeclarative5-controls-plugin is not installed.
  Package qml-module-qtquick-controls is not installed.
 teamviewer depends on qtdeclarative5-dialogs-plugin (>= 5.2) | qml-module-qtquick-dialogs (>= 5.2); however:
  Package qtdeclarative5-dialogs-plugin is not installed.
  Package qml-module-qtquick-dialogs is not installed.

dpkg: error processing package teamviewer (--install):
 dependency problems - leaving unconfigured
Processing triggers for bamfdaemon (0.5.3~bzr0+16.04.20180209-0ubuntu1) ...
Rebuilding /usr/share/applications/bamf-2.index...
Processing triggers for desktop-file-utils (0.22-1ubuntu5.1) ...
Processing triggers for gnome-menus (3.13.3-6ubuntu3.1) ...
Processing triggers for mime-support (3.59ubuntu1) ...
Processing triggers for hicolor-icon-theme (0.15-0ubuntu1) ...
Errors were encountered while processing:
 teamviewer
tom@ud-b:~/Downloads$ sudo apt install -f
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Correcting dependencies... Done
The following packages were automatically installed and are no longer required:
  linux-headers-4.4.0-122 linux-headers-4.4.0-122-generic
  linux-image-4.4.0-122-generic linux-image-extra-4.4.0-122-generic
  linux-signed-image-4.4.0-122-generic
Use 'sudo apt autoremove' to remove them.
The following additional packages will be installed:
  libqt5x11extras5 qml-module-qtquick-controls qml-module-qtquick-dialogs
  qml-module-qtquick-privatewidgets qtdeclarative5-controls-plugin
  qtdeclarative5-dialogs-plugin
The following NEW packages will be installed:
  libqt5x11extras5 qml-module-qtquick-controls qml-module-qtquick-dialogs
  qml-module-qtquick-privatewidgets qtdeclarative5-controls-plugin
  qtdeclarative5-dialogs-plugin
0 upgraded, 6 newly installed, 0 to remove and 24 not upgraded.
1 not fully installed or removed.
Need to get 787 kB of archives.
After this operation, 3,511 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://cn.archive.ubuntu.com/ubuntu xenial/universe amd64 libqt5x11extras5 amd64 5.5.1-3build1 [7,876 B]
Get:2 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 qml-module-qtquick-controls amd64 5.5.1-1ubuntu1 [643 kB]
Get:3 http://cn.archive.ubuntu.com/ubuntu xenial/universe amd64 qtdeclarative5-controls-plugin all 5.5.1-1ubuntu1 [4,032 B]
Get:4 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 qml-module-qtquick-privatewidgets amd64 5.5.1-1ubuntu1 [38.9 kB]
Get:5 http://cn.archive.ubuntu.com/ubuntu xenial/main amd64 qml-module-qtquick-dialogs amd64 5.5.1-1ubuntu1 [89.0 kB]
Get:6 http://cn.archive.ubuntu.com/ubuntu xenial/universe amd64 qtdeclarative5-dialogs-plugin amd64 5.5.1-1ubuntu1 [4,044 B]
Fetched 787 kB in 2s (362 kB/s)                       
Selecting previously unselected package libqt5x11extras5:amd64.
(Reading database ... 276722 files and directories currently installed.)
Preparing to unpack .../libqt5x11extras5_5.5.1-3build1_amd64.deb ...
Unpacking libqt5x11extras5:amd64 (5.5.1-3build1) ...
Selecting previously unselected package qml-module-qtquick-controls:amd64.
Preparing to unpack .../qml-module-qtquick-controls_5.5.1-1ubuntu1_amd64.deb ...
Unpacking qml-module-qtquick-controls:amd64 (5.5.1-1ubuntu1) ...
Selecting previously unselected package qtdeclarative5-controls-plugin.
Preparing to unpack .../qtdeclarative5-controls-plugin_5.5.1-1ubuntu1_all.deb ...
Unpacking qtdeclarative5-controls-plugin (5.5.1-1ubuntu1) ...
Selecting previously unselected package qml-module-qtquick-privatewidgets:amd64.
Preparing to unpack .../qml-module-qtquick-privatewidgets_5.5.1-1ubuntu1_amd64.deb ...
Unpacking qml-module-qtquick-privatewidgets:amd64 (5.5.1-1ubuntu1) ...
Selecting previously unselected package qml-module-qtquick-dialogs:amd64.
Preparing to unpack .../qml-module-qtquick-dialogs_5.5.1-1ubuntu1_amd64.deb ...
Unpacking qml-module-qtquick-dialogs:amd64 (5.5.1-1ubuntu1) ...
Selecting previously unselected package qtdeclarative5-dialogs-plugin:amd64.
Preparing to unpack .../qtdeclarative5-dialogs-plugin_5.5.1-1ubuntu1_amd64.deb ...
Unpacking qtdeclarative5-dialogs-plugin:amd64 (5.5.1-1ubuntu1) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
Setting up libqt5x11extras5:amd64 (5.5.1-3build1) ...
Setting up qml-module-qtquick-controls:amd64 (5.5.1-1ubuntu1) ...
Setting up qtdeclarative5-controls-plugin (5.5.1-1ubuntu1) ...
Setting up qml-module-qtquick-privatewidgets:amd64 (5.5.1-1ubuntu1) ...
Setting up qml-module-qtquick-dialogs:amd64 (5.5.1-1ubuntu1) ...
Setting up qtdeclarative5-dialogs-plugin:amd64 (5.5.1-1ubuntu1) ...
Setting up teamviewer (13.1.8286) ...
Processing triggers for libc-bin (2.23-0ubuntu10) ...
tom@ud-b:~/Downloads$ sudo dpkg -i teamviewer_13.1.8286_amd64.deb 
(Reading database ... 276928 files and directories currently installed.)
Preparing to unpack teamviewer_13.1.8286_amd64.deb ...
Removed symlink /etc/systemd/system/multi-user.target.wants/teamviewerd.service.
Unpacking teamviewer (13.1.8286) over (13.1.8286) ...
Setting up teamviewer (13.1.8286) ...
Processing triggers for bamfdaemon (0.5.3~bzr0+16.04.20180209-0ubuntu1) ...
Rebuilding /usr/share/applications/bamf-2.index...
Processing triggers for desktop-file-utils (0.22-1ubuntu5.1) ...
Processing triggers for gnome-menus (3.13.3-6ubuntu3.1) ...
Processing triggers for mime-support (3.59ubuntu1) ...
Processing triggers for hicolor-icon-theme (0.15-0ubuntu1) ...
tom@ud-b:~/Downloads$ 
```


## Ref

- https://www.cnblogs.com/wmr95/p/7574615.html
- https://www.linuxidc.com/Linux/2017-12/149279.htm

