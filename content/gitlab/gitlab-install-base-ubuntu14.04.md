---
title: "Gitlab Install Base Ubuntu14.04"
date: 2018-04-16T17:36:11+08:00
draft: false
tags: ["gitlab", "ubuntu"]
categories: ["gitlab"]
subtitle: ""
descriptions: ""
bigimg:
gitment: true
---

gitlab在ubuntu14.04和ubuntu16.04上安装

## env

IP: 192.168.31.148
OS: ubuntu-14.04.5
gitlab: 10.6.4

## env2

IP: 192.168.31.169
OS: ubuntu-16.04
gitlab: 10.6.4

## step

按照 [清华大学 Gitlab Community Edition 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/) 安装就可以了.

一定要注意系统版本

修改IP, `sudo vi /etc/gitlab/gitlab.rb`修改下面内容

```
external_url 'http://192.168.31.148:7890'
```
重启
```
sudo gitlab-ctl reconfigure
```

浏览器打开 http://192.168.31.148:7890 修改密码,重新登陆.


## faq

### libstdc++.so.6: version `GLIBCXX_3.4.21' not found

```
strings /home/kzl/anaconda2/bin/../lib/libstdc++.so.6 | grep GLIBCXX    
```
可以看到所有的安装的 glibc++ 版本, 这里确实是没有3.4.21版本

后来才发现,我安装的时候,没有选择正确的系统版本(最先选择了ubuntu16.04,应该先看一下系统版本,然后选择 ubuntu14.04)



## ref

- [Gitlab Community Edition 镜像使用帮助](https://mirror.tuna.tsinghua.edu.cn/help/gitlab-ce/)
- [version `GLIBCXX_3.4.21' not found 解决办法](https://blog.csdn.net/rznice/article/details/51090966)
- [ImportError: /home/kzl/anaconda2/bin/../lib/libstdc++.so.6: version `GLIBCXX_3.4.21' not found](https://blog.csdn.net/lwgkzl/article/details/77658269)