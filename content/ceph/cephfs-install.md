+++
title = "cephfs 安装"
date = 2018-11-30T09:36:00+08:00
lastmod = 2018-11-30T17:11:37+08:00
tags = ["ceph", "cephfs", "install"]
categories = ["ceph"]
draft = false
weight = 3002
+++

## 环境： {#环境}

```
192.168.31.115
192.168.31.114
192.168.31.113
```


## line1 官网, 成功安装 {#line1-官网-成功安装}

我的安装设计是这样的：

```
admin-node, deploy-node(ceph-deploy)：192.168.31.115  cephfs5
mon.0: 192.168.31.114   cephfs4
osd.0: 192.168.31.113   cephfs3
osd.1: 192.168.31.115   cephfs5
mds.0: 192.168.31.113   cephfs3
mds.1: 192.168.31.114   cephfs4
```

还是要结合一下 <https://linux.cn/article-8182-1.html>


## line2 {#line2}

<http://tonybai.com/2017/05/08/mount-cephfs-acrossing-nodes-in-kubernetes-cluster/>


## over {#over}
