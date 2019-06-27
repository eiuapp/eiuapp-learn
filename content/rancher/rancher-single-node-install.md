---
title: "Rancher Single Node Install"
date: 2018-06-17T16:59:39+08:00
draft: false
tags: ["rancher", "kubernetes"]
categories: ["rancher"]
subtitle: "rancher2.0单节点安装kubernetes"
descriptions: ""
bigimg:
---

rancher2.0单节点安装kubernetes
## Env
无论哪个环境

- 硬盘容量
    - 空闲 > 20G
    - 空闲 > 20%

- 有网络

### Env-1(成功)

- cloud: aws
- os: ubuntu16.04
- rancher: v2.0.2
- docker: docker-ce=17.03.2~ce-0~ubuntu-xenial
- post: 8088, 4443

实际上，我是直接把

- ufw: disable

### Env-2(没有成功)可略过。

实验环境

4台vm，配置为2C，4G，100G
一台安装Rancher
一台作为Kubernets cted & control
两台作为Kubernets worker

- os: ubuntu16.04
- rancher: v2.0.2

#### IP

- ip: 192.168.31.189    rancher, win10（192.168.31.102）下的virtualbox
- ip: 192.168.31.188    k8s-etcd, ud-ssd（192.168.31.199）下的virtualbox
- ip: 192.168.31.187    k8s-worker, ud-ssd（192.168.31.199）下的virtualbox
- ip: 192.168.31.186    k8s-worker, ud-putong（192.168.31.112）下的virtualbox

后来发现，192.168.31.186-188 ping不通192.168.31.189。(我三台宿主机全用桥接分配到的IP, 怎么会出现这个情况？）
所以，调整IP如下。

- ip: 192.168.31.199    rancher, win10（192.168.31.102）下的virtualbox
- ip: 192.168.31.199    k8s-etcd, ud-ssd（192.168.31.199）
- ip: 192.168.31.112    k8s-worker, ud-ssd（192.168.31.199）下的virtualbox
- ip: 192.168.31.112    k8s-worker, ud-putong（192.168.31.112）下的virtualbox

### Env-3(成功)

- cloud: 无
- os: ubuntu16.04
- rancher: v2.0.2
- docker: docker-ce=17.03.2~ce-0~ubuntu-xenial
- post: 8088, 4443
- 单硬盘，裸机
- ip: 192.168.31.199

实际上，我是直接把

- ufw: disable

## Step

### 网络互通

所有节点相互之间连通。

### 科学上网

aws下载镜像很快，所以不需要设置。

参考 [透过proxy进行docker pull](https://blog.finsoft.info/posts/docker-pull-with-proxy/)
```
root@uda:/home/tom# tail  /etc/default/docker
#export http_proxy="http://127.0.0.1:3128/"

# This is also a handy place to tweak where Docker's temporary files go.
#export DOCKER_TMPDIR="/mnt/bigdrive/docker-tmp"

HTTP_PROXY=http://192.168.31.112:8118
http_proxy=${HTTP_PROXY}
HTTPS_PROXY=${HTTP_PROXY}
https_proxy=${HTTP_PROXY}
export HTTP_PROXY HTTPS_PROXY http_proxy https_proxy
root@uda:/home/tom#
```

### 所有节点都安装了docker


```
mkdir .tom
cd .tom/
vi docker.sh
chmod +x docker.sh
./docker.sh
```
具体 docker.sh 内容见下方。

```
root@ripple01:~# cat .tom/docker.sh
#!/bin/bash
lsb_release -a
sudo apt-get install     apt-transport-https     ca-certificates     curl     software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
apt-cache madison docker-ce
sudo apt-get install docker-ce=17.03.2~ce-0~ubuntu-xenial -y
sudo docker run hello-world
root@ripple01:~#
```
### 准备基础镜像

这里只是为了加快安装速度，如果你觉得速度没问题，可跳过

当然，如果是rancher版本不是2.0.2，而是其它，这里肯定有些镜像的版本就不是这个了。

所以，如果不是安装2.0.2, 则不要提前准备下面镜像（因为版本不一定是这些嘛），让系统自动下载，只是时间久一点。

#### rancher-server节点

```
docker pull nginx:latest          
docker pull rancher/rancher-agent:v2.0.2          
docker pull rancher/rancher:v2.0.2          
docker pull rancher/rke-tools:v0.1.8          
docker pull rancher/rancher-agent:v2.0.0          
docker pull rancher/rancher:v2.0.0          
docker pull rancher/hyperkube:v1.10.1-rancher2
docker pull rancher/rke-tools:v0.1.4          
docker pull rancher/nginx-ingress-controller:0.10.2-rancher3 
docker pull rancher/calico-node:v3.1.1          
docker pull rancher/calico-cni:v3.1.1          
docker pull hello-world:latest          
docker pull rancher/coreos-etcd:v3.1.12         
docker pull rancher/k8s-dns-dnsmasq-nanny-amd64:1.14.8          
docker pull rancher/k8s-dns-sidecar-amd64:1.14.8          
docker pull rancher/k8s-dns-kube-dns-amd64:1.14.8          
docker pull rancher/pause-amd64:3.1
docker pull rancher/coreos-flannel:v0.9.1          
docker pull rancher/nginx-ingress-controller-defaultbackend:1.4
docker pull rancher/cluster-proportional-autoscaler-amd64:1.0.0
```
如果不想安装这么多，那就至少把`rancher server安装ectd, worker之前`的镜像（下面这些）安装完吧。

```
docker pull nginx:latest
docker pull rancher/hyperkube:v1.10.1-rancher2
docker pull rancher/coreos-etcd:v3.1.12
docker pull rancher/rancher:v2.0.2
docker pull rancher/rancher-agent:v2.0.2
docker pull rancher/rke-tools:v0.1.8
```

如果是`racher2.0.0`则准备把相应镜像的版本号替换下，成下面基础镜像

```
docker pull nginx:latest
docker pull rancher/hyperkube:v1.10.1-rancher2
docker pull rancher/coreos-etcd:v3.1.12
docker pull rancher/rancher:v2.0.0
docker pull rancher/rancher-agent:v2.0.0
docker pull rancher/rke-tools:v0.1.4
```

#### worker节点

```
docker pull nginx:latest
docker pull rancher/rancher-agent:v2.0.2
docker pull rancher/pause-amd64:3.1
docker pull rancher/rke-tools:v0.1.8
docker pull rancher/hyperkube:v1.10.1-rancher2
```
### 运行rancher-server节点

```
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  -v /host/rancher:/var/lib/rancher \
  rancher/rancher:v2.0.2
```

示例如下

```
root@bogon:~# docker run -d --restart=unless-stopped   -p 80:80 -p 443:443   -v /host/rancher:/var/lib/rancher   rancher/rancher:v2.0.2
3aa4d8b39e61a178840196dcd8e05031a0cef56dbcc56b6a72117b7316d19918
root@bogon:~# docker ps
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                                      NAMES
3aa4d8b39e61        rancher/rancher:v2.0.2   "rancher --http-li..."   3 seconds ago       Up 2 seconds        0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   distracted_stallman
root@bogon:~# sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.0.2 --server https://192.168.31.104 --token qt6xd2jg4mvx4qs94q8bqlfdqcdgr6p794cwzj5vhld2l62ds6l9lw --ca-checksum 1d2d1038b6e78cd0c4925bd5931629e6e6e0d9c97699cdbabe9eb59448154f85 --etcd --controlplane --worker
Unable to find image 'rancher/rancher-agent:v2.0.2' locally
v2.0.2: Pulling from rancher/rancher-agent
a48c500ed24e: Already exists
1e1de00ff7e1: Already exists
0330ca45a200: Already exists
471db38bcfbf: Already exists
0b4aba487617: Already exists
b0300e97030b: Downloading [========>                                          ]  2.87 MB/17.59 MB
64b8ae7ae0d5: Download complete
73facef3793d: Downloading [=============>                                     ] 3.521 MB/12.97 MB
649b24d0dfef: Downloading [=================>                                 ]   3.8 MB/10.86 MB
```

### 浏览器设置

设置密码，拿到docker命令
### rancher-server, etcd, control, worker节点运行container

注意：

- 这里，每个人的 IP, port, token, 启动项目类型不同，以浏览器内容为准，不要复制下面的。
- 如果后续要加work, 记得不要再启动 etcd, controlplane

基本流程就是这样了。

## 命令汇总

下面可以看一下，我自己的实际操作的命令汇总吧。

在 aws 环境下，成功安装了rancher，命令汇总如下

#### 查看一下基本信息，知道的，可以忽略

```
sudo -i
ufw status
which docker
lsb_release -a
top
cat /proc/cpuinfo
df -h
```
#### 安装docker

```
mkdir .tom
cd .tom/
vi docker.sh
chmod +x docker.sh
./docker.sh
```
#### 安装准备镜像

```
docker pull nginx:latest
docker pull rancher/hyperkube:v1.10.1-rancher2
docker pull rancher/coreos-etcd:v3.1.12
docker pull rancher/rancher:v2.0.2
docker pull rancher/rancher-agent:v2.0.2
docker pull rancher/rke-tools:v0.1.8
## 如果不安装v2.0.0则不需要下面2行
# docker pull rancher/rancher:v2.0.0
# docker pull rancher/rke-tools:v0.1.4
```
备注：

这个操作是只有这么几个镜像？实际上应该如下面这样的。但是，aws网络实在是好，其它的，都它自己下载了。

```
docker pull nginx:latest          
docker pull rancher/rancher-agent:v2.0.2          
docker pull rancher/rancher:v2.0.2          
docker pull rancher/rke-tools:v0.1.8          
docker pull rancher/rancher-agent:v2.0.0          
docker pull rancher/rancher:v2.0.0          
docker pull rancher/hyperkube:v1.10.1-rancher2
docker pull rancher/rke-tools:v0.1.4          
docker pull rancher/nginx-ingress-controller:0.10.2-rancher3 
docker pull rancher/calico-node:v3.1.1          
docker pull rancher/calico-cni:v3.1.1          
docker pull hello-world:latest          
docker pull rancher/coreos-etcd:v3.1.12         
docker pull rancher/k8s-dns-dnsmasq-nanny-amd64:1.14.8          
docker pull rancher/k8s-dns-sidecar-amd64:1.14.8          
docker pull rancher/k8s-dns-kube-dns-amd64:1.14.8          
docker pull rancher/pause-amd64:3.1
docker pull rancher/coreos-flannel:v0.9.1          
docker pull rancher/nginx-ingress-controller-defaultbackend:1.4
docker pull rancher/cluster-proportional-autoscaler-amd64:1.0.0
```
#### 启动rancher-server

把rancher-server持久化到本机 `/host/rancher`。

注意设置port
```
docker run -d --restart=unless-stopped -p 8088:80 -p 4443:443 -v /host/rancher:/var/lib/rancher   rancher/rancher:v2.0.2
docker ps
```

#### 打开浏览器

设置admin密码，并拿到docker命令
#### 启动，etcd，controlplane，worker

注意：

- 这里，每个人的 IP, port, token, 启动项目类型不同，以浏览器内容为准，不要复制下面的。
- 如果后续要加work, 记得不要再启动 etcd, controlplane

```
docker images
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.0.2 --server https://yourIP:4443 --token d82dc2g4qbhvxwssm95sf6mtl2plxgk4jmmx4xnj6zd2thsnjt9sq9 --ca-checksum c160ff8a67d540af15c3cf7299e65a81b99f476b9e06526d39ada649afbd1f67 --etcd --controlplane --worker
docker ps
```

[^_^]:

    ```
    docker images
    sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.0.2 --server https://47.52.242.6:4443 --token d82dc2g4qbhvxwssm95sf6mtl2plxgk4jmmx4xnj6zd2thsnjt9sq9 --ca-checksum c160ff8a67d540af15c3cf7299e65a81b99f476b9e06526d39ada649afbd1f67 --etcd --controlplane --worker
    docker ps
    ```

回浏览器查看

- cluster状态: `Active`
- node状态: `Active`
- node下的`Disk Space`，`Disk Pressure`，`Memory Pressure`，`Kubelet`：绿色，打勾
- cluster下`Launch kubectl`正常可打开，且，运行`kubectl get all`返回正常

至此，就知道了,所有成功了,完成了。


## FAQ

### cni config uninitialized

Q: Env-2环境下，这个时候，会出现 https://github.com/rancher/rancher/issues/13484 的报错信息
A: 这个问题，因为在Env-1(aws)环境下，没有出现，也就意味着，很可能是网络下载环节出了问题（可能是下载cni相关镜像出错了，比如网络连接中断）

### docker images

Q: 准备阶段，为什么是那些镜像呢？

A: 我这里主要是依据，在 aws 成功安装rancher server 并启动一个kubernetes后, 通过查看 `docker images` 得到。

```
root@ripple01:~# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
nginx                                             latest              cd5239a0906a        12 days ago         109 MB
rancher/rancher-agent                             v2.0.2              df087865517d        3 weeks ago         228 MB
rancher/rancher                                   v2.0.2              88526c7bea4e        3 weeks ago         521 MB
rancher/rke-tools                                 v0.1.8              5df9ccc4e588        4 weeks ago         136 MB
rancher/rancher-agent                             v2.0.0              8cfec7659f1d        6 weeks ago         248 MB
rancher/rancher                                   v2.0.0              3141e5c66ee8        6 weeks ago         535 MB
rancher/hyperkube                                 v1.10.1-rancher2    3e3f2ccada54        7 weeks ago         967 MB
rancher/rke-tools                                 v0.1.4              d1a7302844b3        7 weeks ago         56.9 MB
rancher/nginx-ingress-controller                  0.10.2-rancher3     66e10c24a484        7 weeks ago         203 MB
rancher/calico-node                               v3.1.1              d94b64ac210d        8 weeks ago         248 MB
rancher/calico-cni                                v3.1.1              482f47df27e2        8 weeks ago         68.8 MB
hello-world                                       latest              e38bc07ac18e        2 months ago        1.85 kB
rancher/coreos-etcd                               v3.1.12             02f30926cd60        3 months ago        34.8 MB
rancher/k8s-dns-dnsmasq-nanny-amd64               1.14.8              c2ce1ffb51ed        5 months ago        41 MB
rancher/k8s-dns-sidecar-amd64                     1.14.8              6f7f2dc7fab5        5 months ago        42.2 MB
rancher/k8s-dns-kube-dns-amd64                    1.14.8              80cc5ea4b547        5 months ago        50.5 MB
rancher/pause-amd64                               3.1                 da86e6ba6ca1        5 months ago        742 kB
rancher/coreos-flannel                            v0.9.1              2b736d06ca4c        7 months ago        51.3 MB
rancher/nginx-ingress-controller-defaultbackend   1.4                 846921f0fe0e        7 months ago        4.84 MB
rancher/cluster-proportional-autoscaler-amd64     1.0.0               e183460c484d        19 months ago       48.2 MB
root@ripple01:~#
```

可知，我们真正要下载的images 有 下面这些images

```
docker pull nginx:latest          
docker pull rancher/rancher-agent:v2.0.2          
docker pull rancher/rancher:v2.0.2          
docker pull rancher/rke-tools:v0.1.8          
docker pull rancher/rancher-agent:v2.0.0          
docker pull rancher/rancher:v2.0.0          
docker pull rancher/hyperkube:v1.10.1-rancher2
docker pull rancher/rke-tools:v0.1.4          
docker pull rancher/nginx-ingress-controller:0.10.2-rancher3 
docker pull rancher/calico-node:v3.1.1          
docker pull rancher/calico-cni:v3.1.1          
docker pull hello-world:latest          
docker pull rancher/coreos-etcd:v3.1.12         
docker pull rancher/k8s-dns-dnsmasq-nanny-amd64:1.14.8          
docker pull rancher/k8s-dns-sidecar-amd64:1.14.8          
docker pull rancher/k8s-dns-kube-dns-amd64:1.14.8          
docker pull rancher/pause-amd64:3.1
docker pull rancher/coreos-flannel:v0.9.1          
docker pull rancher/nginx-ingress-controller-defaultbackend:1.4
docker pull rancher/cluster-proportional-autoscaler-amd64:1.0.0
```

当然，如果是rancher版本不是2.0.2，而是其它，这里肯定有些镜像的版本就不是这个了。

所以，如果不是安装2.0.2, 则，不提前准备镜像，让系统，自动下载，只是时间久一点。


### 安装完成后

Q: 安装完成v2.0.2后，安装etcd, controlplane, worker, 共下载多少镜像？共启动多少container?

A: 看下面

```
root@ud-b:~# docker images
REPOSITORY                                        TAG                 IMAGE ID            CREATED             SIZE
rancher/rancher-agent                             v2.0.2              df087865517d        3 weeks ago         228 MB
rancher/rancher                                   v2.0.2              88526c7bea4e        3 weeks ago         521 MB
rancher/rke-tools                                 v0.1.8              5df9ccc4e588        4 weeks ago         136 MB
rancher/hyperkube                                 v1.10.1-rancher2    3e3f2ccada54        7 weeks ago         967 MB
rancher/nginx-ingress-controller                  0.10.2-rancher3     66e10c24a484        7 weeks ago         203 MB
rancher/calico-node                               v3.1.1              d94b64ac210d        2 months ago        248 MB
rancher/calico-cni                                v3.1.1              482f47df27e2        2 months ago        68.8 MB
rancher/coreos-etcd                               v3.1.12             02f30926cd60        3 months ago        34.8 MB
rancher/k8s-dns-dnsmasq-nanny-amd64               1.14.8              c2ce1ffb51ed        5 months ago        41 MB
rancher/k8s-dns-sidecar-amd64                     1.14.8              6f7f2dc7fab5        5 months ago        42.2 MB
rancher/k8s-dns-kube-dns-amd64                    1.14.8              80cc5ea4b547        5 months ago        50.5 MB
rancher/pause-amd64                               3.1                 da86e6ba6ca1        6 months ago        742 kB
rancher/coreos-flannel                            v0.9.1              2b736d06ca4c        7 months ago        51.3 MB
rancher/nginx-ingress-controller-defaultbackend   1.4                 846921f0fe0e        7 months ago        4.84 MB
rancher/cluster-proportional-autoscaler-amd64     1.0.0               e183460c484d        19 months ago       48.2 MB
root@ud-b:~# docker images | wc -l
16
root@ud-b:~# docker ps | wc -l
26
root@ud-b:~#
```

所以，共下载16个镜像，启动26个容器。

### kubectl get pods --all-namespaces

Q: 为什么`kubectl get pods --all-namespaces`中的`ingress-nginx`是`CrashLoopBackOff`状态，具体如下？

```
# Run kubectl commands inside here
# e.g. kubectl get all
> kubectl get pods --all-namespaces
NAMESPACE       NAME                                    READY     STATUS             RESTARTS   AGE
cattle-system   cattle-cluster-agent-8d7c4cf9b-7f6jb    1/1       Running            1          4h
cattle-system   cattle-node-agent-r29z4                 1/1       Running            0          4h
ingress-nginx   default-http-backend-564b9b6c5b-4k2x6   1/1       Running            0          4h
ingress-nginx   nginx-ingress-controller-jlcbt          0/1       CrashLoopBackOff   60         4h
kube-system     canal-2k7qn                             3/3       Running            0          4h
kube-system     kube-dns-5ccb66df65-d8qrb               3/3       Running            0          4h
kube-system     kube-dns-autoscaler-6c4b786f5-xj7nj     1/1       Running            0          4h
>
```

A: 这个谁知道的？求留言。

### node出现`Disk Pressure`

Q: 出现`Disk Pressure`, 是什么问题？

A: 硬盘空间不够。最少要有20G或者总硬盘空间的20%。

我有一次就是在硬盘空间这里卡住了，导致没有继续往下安装。

### 安装时可以使用加速器么？

Q: 安装时可以使用加速器么？

A: 可以使用，且建议使用。

具体如何使用，可以参考 https://yq.aliyun.com/articles/29941

### 删除rancher

Q: 如何删除rancher?

A: 其实就是删除所有相关容器，那么直接参考[高人的sh](https://gist.github.com/superseb/2cf186726807a012af59a027cb41270d)就行了。

```
root@uda:~# cat clean-up.sh
#!/bin/sh
docker rm -f $(docker ps -qa)
docker volume rm $(docker volume ls -q)
cleanupdirs="/var/lib/etcd /etc/kubernetes /etc/cni /opt/cni /var/lib/cni /var/run/calico"
for dir in $cleanupdirs; do
  echo "Removing $dir"
  rm -rf $dir
done
root@uda:~#
```
运行上面这个`clean-up.sh`

如果要连rancher的持久化数据也删除，则把当时创建的持久化数据文件 `/host/rancher/`也删除吧。

## Ref

- https://github.com/rancher/rancher/issues/13484
- https://gist.github.com/superseb/2cf186726807a012af59a027cb41270d
- https://rancher.com/docs/rancher/v2.x/en/quick-start-guide/
- https://rancher.com/docs/rancher/v2.x/en/installation/single-node-install/
- https://yq.aliyun.com/articles/29941
- https://www.cnblogs.com/xzkzzz/p/9106218.html
