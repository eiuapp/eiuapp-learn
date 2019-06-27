---
title: "Ubuntu Apt Http Proxy"
date: 2018-05-24T10:16:52+08:00
draft: false
tags: ["ubuntu", "proxy"]
categories: ["ubuntu"]
subtitle: ""
descriptions: "Ubuntu的更改apt-get代理设置"
bigimg:
---

Ubuntu的更改apt-get代理，设置与取消

## apt代理的设置

#### 方法1

这是一种临时的手段，如果您仅仅是暂时需要通过http代理使用apt-get，您可以使用这种方式。

在使用apt-get之前，在终端中输入以下命令（根据您的实际情况替换yourproxyaddress和proxyport）。

```
export http_proxy=http://yourproxyaddress:proxyport
```
#### 方法2

这种方法要用到/etc/apt/文件夹下的apt.conf文件。如果您希望apt-get（而不是其他应用程序）一直使用http代理，您可以使用这种方式。

注意：某些情况下，系统安装过程中没有建立apt配置文件。下面的操作将视情况修改现有的配置文件或者新建配置文件。

sudo gedit /etc/apt/apt.conf在您的apt.conf文件中加入下面这行（根据你的实际情况替换yourproxyaddress和proxyport）。

```
Acquire::http::Proxy "http://yourproxyaddress:proxyport"
```
保存apt.conf文件。

#### 方法3

这种方法会在您的主目录下的.bashrc文件中添加两行。如果您希望apt-get和其他应用程序如wget等都使用http代理，您可以使用这种方式。

gedit ~/.bashrc在您的.bashrc文件末尾添加如下内容（根据你的实际情况替换yourproxyaddress和proxyport）。
```
http_proxy=http://yourproxyaddress:proxyport
```
export http_proxy保存文件。关闭当前终端，然後打开另一个终端。

使用apt-get update或者任何您想用的网络工具测试代理。我使用firestarter查看活动的网络连接。

如果您为了纠正错误而再次修改了配置文件，记得关闭终端并重新打开，否自新的设置不会生效。


#### 方法4

另外，apt-get也有一个“-o”选项，直接跟apt-get的设置变量，就不用指定配置文件了，比如

```
sudo apt-get -o Acquire::http::proxy="http://127.0.0.1:1080/”
```

## 取消apt代理

```
今天想装个软件(wine)，使用 sudo apt-get update 命令时，发现给出很多Ign 语句，总出现 Connecting to proxy.http://10.0.126.1:13128 的字样，发现这个代理是已经废弃掉的。接着想去取消使用该代理：
　　1、 查看/etc/apt/apt.conf，发现存在：
　　http_proxy="http://10.0.126.1:13128/"
　　https_proxy="https://10.0.126.1:13128/"
　　ftp_proxy="ftp://10.0.126.1:13128/"
　　socks_proxy="socks://10.0.126.1:13128/"
　　直接删除该文件，重启电脑，发现问题还是没解决；
　　2、百度一下，命令行执行：export  http_proxy="" 发现问题未解；
　　执行 unset  http_proxy 问题还是存在；
　　3、查看~/.bashrc，未发现存在http_proxy之类设置;
　　4、env | grep proxy 发现依然存在 http 代理；
　　5、根目录查找一把： sudo grep -r -i http_proxy=http://10.0.126.168:13128/ ./
　　看到控制台有输出: /etc/enviroment : http_proxy .....
　　6、查看一下：cat /etc/enviroment，发现有配置：
　　http_proxy="http://10.0.126.1:13128/"
　　https_proxy="https://10.0.126.1:13128/"
　　tp_proxy="ftp://10.0.126.1:13128/"
　　socks_proxy="socks://10.0.126.1:13128/"
　　7、vi /etc/environment 将配置删掉；
　　8、至此终于搞定然后  sudo apt-get UPDATE,出现n多更新。
　　记录一下。
```

## ref

- https://blog.csdn.net/yctccg/article/details/52218066
- https://blog.csdn.net/huangkq1989/article/details/9314749
- https://www.iyunv.com/thread-206089-1-1.html
- https://blog.csdn.net/huangkq1989/article/details/9314749