---
title: "Static Ip Ubuntu18"
date: 2018-06-16T21:52:43+08:00
draft: false
tags: ["ubuntu", "ip"]
categories: ["ip"]
subtitle: "ubuntu18设置固态IP"
descriptions: ""
bigimg:
---

ubuntu18设置固态IP

## Env

- os: ubuntu18

## Step

### ubuntu16

ubuntu16是在`/etc/network/interfaces` 文件中设置固态IP
设置的方式类似下面

```
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto ens160
#iface ens160 inet dhcp
iface ens160 inet static
address 192.168.31.120
gateway 192.168.31.1 #这个地址你要确认下 网关是不是这个地址
netmask 255.255.255.0
network 192.168.31.0
broadcast 192.168.31.255
dns-nameservers 202.96.134.133
```

### ubuntu17及以上

但是，ubuntu17起，改成了`etc/netplan/*.yaml`来配置。

配置方式类似下面

```
tom@tom:~$ cat /etc/netplan/50-cloud-init.yaml
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    version: 2
    ethernets:
        enp0s3:
            dhcp4: no
            dhcp6: no
            addresses: [192.168.31.103/24, '2001:1::2/64']
            gateway4: 192.168.31.1
            nameservers:
                addresses: [192.168.31.1]
tom@tom:~$
```

配置完成后，运行
```
sudo netplan apply
```
此时，固态IP配置完成。


## Ref

- https://websiteforstudents.com/configuring-static-ips-ubuntu-17-10-servers/