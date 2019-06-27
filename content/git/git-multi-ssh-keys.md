+++
title = "配置公私钥别名"
date = 2018-11-13T22:10:00+08:00
lastmod = 2018-11-26T18:19:30+08:00
tags = ["git"]
categories = ["git"]
draft = false
weight = 3001
+++

## 配置公私钥别名 {#配置公私钥别名}


### 私钥权限不是600 {#私钥权限不是600}

```shell
➜  tom-finsoftinfo git:(master) ✗ ssh-add id_rsa_finsoftinfo
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0644 for 'id_rsa_finsoftinfo' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
➜  tom-finsoftinfo git:(master) ✗

```

看提示，就知道是权限不对了。修改为600，就可以了。

```shell
➜  tom-finsoftinfo git:(master) ✗ l
total 16K
drwxr-xr-x 6 tomtsang  192 Nov 13 22:56 .
drwx--x--x 7 tomtsang  224 Oct 15 11:20 ..
-rw-r--r-- 1 tomtsang 1.7K Nov 13 22:56 id_rsa_finsoftinfo
-rw-r--r-- 1 tomtsang  399 Nov 13 22:56 id_rsa_finsoftinfo.pub
➜  tom-finsoftinfo git:(master) ✗ chmod 600 id_rsa_finsoftinfo
➜  tom-finsoftinfo git:(master) ✗ l
total 16K
drwxr-xr-x 6 tomtsang  192 Nov 13 22:56 .
drwx--x--x 7 tomtsang  224 Oct 15 11:20 ..
-rw------- 1 tomtsang 1.7K Nov 13 22:56 id_rsa_finsoftinfo
-rw-r--r-- 1 tomtsang  399 Nov 13 22:56 id_rsa_finsoftinfo.pub
➜  tom-finsoftinfo git:(master) ✗ ssh-add id_rsa_finsoftinfo
Identity added: id_rsa_finsoftinfo (id_rsa_finsoftinfo)
➜  tom-finsoftinfo git:(master) ✗ ssh-add -l
2048 SHA256:KnwSrbgqEaN9EZdTrfME/KJCDvMdATgdzS9GVHV9wBY id_rsa_finsoftinfo (RSA)
➜  tom-finsoftinfo git:(master) ✗ ssh -T finsoftinfo
Hi finsoftinfo! You've successfully authenticated, but GitHub does not provide shell access.
➜
```

```shell
➜  .ssh git:(master) ✗ cd /Users/tomtsang/github/finsoftinfo/finsoftinfo.github.io
➜  finsoftinfo.github.io git:(master) g pull finsoftinfo master
ERROR: Repository not found.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
➜  finsoftinfo.github.io git:(master) g remote -v
finsoftinfo	finsoftinfo:finsoftinfo.github.io (fetch)
finsoftinfo	finsoftinfo:finsoftinfo.github.io (push)
➜  finsoftinfo.github.io git:(master)

```


## Could not read from remote repository {#could-not-read-from-remote-repository}


### 别名的仓库url 不对 {#别名的仓库url-不对}

```shell
➜  finsoftinfo.github.io git:(master) pwd
/Users/tomtsang/github/finsoftinfo/finsoftinfo.github.io
➜  finsoftinfo.github.io git:(master) g pull finsoftinfo master
ERROR: Repository not found.
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
➜  finsoftinfo.github.io git:(master)
```

注意看一下仓库名，修改正确就行了。这里特别注意一下，总共出现了三次的\`finsoftinfo\`

```shell
➜  finsoftinfo.github.io git:(master) g remote -v
finsoftinfo	finsoftinfo:finsoftinfo.github.io (fetch)
finsoftinfo	finsoftinfo:finsoftinfo.github.io (push)
➜  finsoftinfo.github.io git:(master) g remote set-url finsoftinfo finsoftinfo:finsoftinfo/finsoftinfo.github.io
➜  finsoftinfo.github.io git:(master) g remote -v
finsoftinfo	finsoftinfo:finsoftinfo/finsoftinfo.github.io (fetch)
finsoftinfo	finsoftinfo:finsoftinfo/finsoftinfo.github.io (push)
➜  finsoftinfo.github.io git:(master)
```
