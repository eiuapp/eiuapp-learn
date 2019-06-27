---
title: "How to Install Certificates for Command Line"
date: 2018-06-21T22:32:14+08:00
draft: false
tags: ["CA", "ubuntu", "download"]
categories: ["CA"]
subtitle: "命令行安装CA.crt文件, 从而下载文件"
descriptions: ""
bigimg:
---

当你有一个 CA.crt 文件时，怎么安装它，然后使用呢？我遇到了这个问题。

## Env

- os: ubuntu16
- 192.168.31.120

## Step

尝试直接 apt install

    jlch@km:~$ sudo apt install ./CA.crt
    [sudo] password for jlch:
    Reading package lists... Done
    E: Unsupported file ./CA.crt given on commandline
    jlch@km:~$

不行，那尝试一下

    jlch@km:~$ dpkg-query -L ca-certificates
    /.
    /etc
    /etc/ssl
    /etc/ssl/certs
    /etc/ca-certificates
    /etc/ca-certificates/update.d
    /usr
    /usr/sbin
    /usr/sbin/update-ca-certificates
    /usr/share
    /usr/share/ca-certificates
    /usr/share/ca-certificates/mozilla
    ... # 看到了好多 mozilla 的证书相关信息呀。
    /usr/share/ca-certificates/mozilla/DigiCert_Global_Root_CA.crt
    /usr/share/doc
    /usr/share/doc/ca-certificates
    /usr/share/doc/ca-certificates/examples
    ... # 好多 examples
    /usr/share/doc/ca-certificates/examples/ca-certificates-local/README
    /usr/share/doc/ca-certificates/changelog.gz
    /usr/share/doc/ca-certificates/copyright
    /usr/share/doc/ca-certificates/README.Debian
    /usr/share/doc/ca-certificates/NEWS.Debian.gz
    /usr/share/man
    /usr/share/man/man8
    /usr/share/man/man8/update-ca-certificates.8.gz
    jlch@km:~$

那查一下 update-ca-certificates

    jlch@km:~$ which update-ca-certificates
    jlch@km:~$ sudo which update-ca-certificates
    [sudo] password for jlch:
    /usr/sbin/update-ca-certificates
    jlch@km:~$

好了, 根据
<https://askubuntu.com/questions/645818/how-to-install-certificates-for-command-line>
来更新吧

    jlch@km:~$ cd /usr/sbin/update-ca-certificates
    jlch@km:/usr/share/ca-certificates/xxnet$ sudo cp /home/jlch/CA.crt .
    jlch@km:/usr/share/ca-certificates/xxnet$ sudo /usr/sbin/update-ca-certificates
    Updating certificates in /etc/ssl/certs...
    0 added, 0 removed; done.
    Running hooks in /etc/ca-certificates/update.d...
    done.
    jlch@km:/usr/share/ca-certificates/xxnet$

好了，CA证书已经更新成功了。

### 测试

    jlch@km:~$ export http_proxy=http://192.168.31.10:8087
    jlch@km:~$ curl -sSL https://dl.k8s.io/release/stable.txt
    jlch@km:~$

看吧，通过这个 CA.crt 文件，从指定的网站 https://dl.k8s.io/release/stable.txt 下载了这个 stable.txt 文件哟。

## Ref

- https://askubuntu.com/questions/645818/how-to-install-certificates-for-command-line