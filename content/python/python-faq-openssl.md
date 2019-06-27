---
title: "Python Faq Openssl"
date: 2018-06-13T13:00:01+08:00
draft: false
tags: ["linux", "python", "openssl", "faq"]
categories: ["linux"]
subtitle: "linux中修复`fatal error: openssl/aes.h: No such file or directory`"
descriptions: ""
bigimg:
---

## Env

- os: ubuntu16

#### 出错

```
/alg -Iscrypt-1.2.1/libcperciva/util -Iscrypt-1.2.1/libcperciva/crypto -I/usr/local/include -I/usr/include -I/usr/include/python2.7 -c scrypt-1.2.1/libcperciva/crypto/crypto_aes.c -o build/temp.linux-x86_64-2.7/scrypt-1.2.1/libcperciva/crypto/crypto_aes.o -O2
    scrypt-1.2.1/libcperciva/crypto/crypto_aes.c:6:25: fatal error: openssl/aes.h: No such file or directory
    compilation terminated.
    error: command 'x86_64-linux-gnu-gcc' failed with exit status 1

    ----------------------------------------
Command "/usr/bin/python -u -c "import setuptools, tokenize;__file__='/tmp/pip-install-qN0XHo/scrypt/setup.py';f=getattr(tokenize, 'open', open)(__file__);code=f.read().replace('\r\n', '\n');f.close();exec(compile(code, __file__, 'exec'))" install --record /tmp/pip-record-8LxwEK/install-record.txt --single-version-externally-managed --compile" failed with error code 1 in /tmp/pip-install-qN0XHo/scrypt/
root@ip-172-31-23-102:/home/ubuntu/rpc#
```
## Step

看错误。这里看的错误，不应该是`error: command 'x86_64-linux-gnu-gcc' failed with exit status 1`,而应该是`fatal error: openssl/aes.h: No such file or directory`。（之前就是犯了这个错误，而弄错了查错方向）

如果你在编译时遇到这个错误，这可能是下面的原因：你尝试编译的程序使用OpenSSL，但是需要和OpenSSL链接的文件（库和头文件）在你Linux平台上缺少。（译注：其它类似的错误也可以照此处理）

要解决这个问题，你需要安装OpenSSL 开发包，这在所有的现代Linux发行版的标准软件仓库中都有。

要在Debian、Ubuntu或者其他衍生版上安装OpenSSL：

```
sudo apt-get install libssl-dev
```

要在Fedora、CentOS或者RHEL上安装OpenSSL开发包：

```
sudo yum install openssl-devel
```
安装完后，尝试重新编译程序。

## Ref
- [Linux 有问必答：如何修复“fatal error: openssl/aes.h: No such file or directory](https://linux.cn/article-4147-1.html)
- http://ask.xmodulo.com/fix-fatal-error-openssl.html