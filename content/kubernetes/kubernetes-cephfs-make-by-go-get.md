+++
title = "kubernetes-cephfs-make-by-go-get"
date = 2018-11-25T17:35:00+08:00
lastmod = 2018-11-25T17:41:05+08:00
tags = ["kubernetes", "ceph", "cephfs", "make"]
categories = ["kubernetes"]
draft = false
weight = 3004
+++

## ENV {#env}

```
k8s-master 192.168.31.120 km master
k8s-node1 192.168.31.119 kn1 node1
k8s-node2 192.168.31.118 kn2 node2
ceph-client 192.168.31.172
ceph-mon1 192.168.31.114
```

这次的make, 可以在任何地方完成，只要满足：golang 1.7 以上的版本

我在 km,ceph-client,ceph-mon1 上都完成过


## 安装golang {#安装golang}

如果已有安装，请忽略这一步

安装 golang 1.7 以上的版本。 我们这里安装 1.9.1

> cd _home/jlch_
> tar -xvf go1.9.2.linux-amd64.tar
> ls go
> export PATH=$PATH:/home/jlch/go/bin


## 验证go {#验证go}

```
go version
```


## 配置 GOPATH {#配置-gopath}

```
mkdir gopath
export GOPATH=/home/jlch/gopath/
```


## go get {#go-get}

```
go get github.com/kubernetes-incubator/external-storage
```


## 配置 Dockerfile {#配置-dockerfile}

后来发现 docker image 的文件不对。

这个地方的 ENV CEPH\_VERSION "jewel" 应该修改成 ENV CEPH\_VERSION "luminous"

然后再 make

```
cd /github.com/kubernetes-incubator/external-storage/ceph/cephfs/
vi Dockerfile
```

看一下。

```
root@km:~/cephfs# cat ~/kubernetes.io/TUTORIALS/Stateful-Applications/cephfs-stateful/external-storage/ceph/cephfs/Dockerfile
# Copyright 2017 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM centos:7

ENV CEPH_VERSION "jewel"
RUN rpm -Uvh https://download.ceph.com/rpm-$CEPH_VERSION/el7/noarch/ceph-release-1-1.el7.noarch.rpm && \
yum install -y epel-release && \
yum install -y ceph-common python-cephfs

COPY cephfs-provisioner /usr/local/bin/cephfs-provisioner
COPY cephfs_provisioner/cephfs_provisioner.py /usr/local/bin/cephfs_provisioner
CMD ["chmod", "o+x", "/usr/local/bin/cephfs_provisioner"]
root@km:~/cephfs#
```


## make {#make}

```
cd gopath/src/
cd ./github.com/kubernetes-incubator/external-storage/
```

这里如果 make ceph/cephfs/ 直接这么走，会报如下错误。

```
[tom@test_240 external-storage]$ make ceph/cephfs/
make: 对“ceph/cephfs/”无需做任何事。
[tom@test_240 external-storage]$
```

所以要 cd ceph/cephfs/ && make ，如下：

```
[tom@test_240 external-storage]$ cd ceph/cephfs/
[tom@test_240 cephfs]$ ls
cephfs_provisioner     ceph-secret-admin.yaml  claim.yaml  configmap.yaml   Dockerfile      Makefile  README.md
cephfs-provisioner.go  CHANGELOG.md            class.yaml  deployment.yaml  local-start.sh  OWNERS    test-pod.yaml
[tom@test_240 cephfs]$ make
CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -o cephfs-provisioner cephfs-provisioner.go
[tom@test_240 cephfs]$
```

这个时候，在 ceph/cephfs/ 下会多出一个 cephfs-provisioner 文件

```
[tom@test_240 cephfs]$ ls
cephfs_provisioner  cephfs-provisioner  cephfs-provisioner.go  ceph-secret-admin.yaml  CHANGELOG.md  claim.yaml  class.yaml  configmap.yaml  deployment.yaml  Dockerfile  local-start.sh  Makefile  OWNERS  README.md  test-pod.yaml
[tom@test_240 cephfs]$
```


## make push {#make-push}

生成 docker image , quay.io/external\_storage/cephfs-provisioner

```
make push
```

如果说出现下面这个样子，说明是 make 成了 docker image了，但是 Push没有成功（应该是指 push 到 docker.io 没有成功）

```
79c182856123: Preparing
cf516324493c: Preparing
unauthorized: access to the requested resource is not authorized
make: *** [push] 错误 1
[tom@test_240 cephfs]$
```


## push 到 registry {#push-到-registry}

因为有 reg.jlch.com:5000 这个 registry 了，先登录

```
docker login reg.jlch.com:5000
docker tag quay.io/external_storage/cephfs-provisioner:latest reg.jlch.com:5000/quay.io/external_storage/cephfs-provisioner:20171114
docker push reg.jlch.com:5000/quay.io/external_storage/cephfs-provisioner:20171114
```

删除

```
docker rmi reg.jlch.com:5000/quay.io/external_storage/cephfs-provisioner:20171114
```


## game over {#game-over}
