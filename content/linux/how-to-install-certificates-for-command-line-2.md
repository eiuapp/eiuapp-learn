+++
title = "命令行下如何安装或者更新 CA.crt 文件"
date = 2018-11-29T15:50:00+08:00
lastmod = 2018-11-30T09:13:32+08:00
tags = ["ubuntu", "certificates"]
categories = ["ubuntu"]
draft = false
weight = 3002
+++

<!--more-->

<https://askubuntu.com/questions/645818/how-to-install-certificates-for-command-line>


## env {#env}

192.168.31.120


## step {#step}

尝试直接 apt install

```
jlch@km:~$ sudo apt install ./CA.crt
[sudo] password for jlch:
Reading package lists... Done
E: Unsupported file ./CA.crt given on commandline
jlch@km:~$
```

不行，那尝试一下

```
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
```

那查一下 update-ca-certificates

```
jlch@km:~$ which update-ca-certificates
jlch@km:~$ sudo which update-ca-certificates
[sudo] password for jlch:
/usr/sbin/update-ca-certificates
jlch@km:~$
```

好了, 根据
<https://askubuntu.com/questions/645818/how-to-install-certificates-for-command-line>
来更新吧

```
jlch@km:~$ cd /usr/sbin/update-ca-certificates
jlch@km:/usr/share/ca-certificates/xxnet$ sudo cp /home/jlch/CA.crt .
jlch@km:/usr/share/ca-certificates/xxnet$ sudo /usr/sbin/update-ca-certificates
Updating certificates in /etc/ssl/certs...
0 added, 0 removed; done.
Running hooks in /etc/ca-certificates/update.d...
done.
jlch@km:/usr/share/ca-certificates/xxnet$
```

好了，CA证书已经更新成功了。


## 测试 {#测试}

```
jlch@km:~$ export http_proxy=http://192.168.31.10:8087
jlch@km:~$ curl -sSL https://dl.k8s.io/release/stable.txt
jlch@km:~$
```
