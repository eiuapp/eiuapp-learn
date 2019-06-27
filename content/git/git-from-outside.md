---
title: "Git From Outside"
date: 2018-03-02T11:37:44+08:00
draft: false
tags: ["git"]
categories: ["git"]
subtitle: ""
descriptions: ""
bigimg:
---

从外部复制一个.git文件，如何使用

## 问题

有.git 文件　 ，直接使用，报错如下：

    fatal: '/srv/OpenX.git' does not appear to be a git repository
    fatal: Could not read from remote repository.

现在有一个新来的.git文件, 如：OpenX.git

1. 放到本机的git仓库目录

        [tom@check repositories]$ sudo cp -a OpenX.git/ /srv/
2. 建立新的下传使用目录

        [tom@check repositories]$ cd ../testgit/
        [tom@check testgit]$ mkdir testgit2
        [tom@check testgit]$ ls
        OpenXtest  testgit2
        [tom@check testgit]$ cd testgit2/
        [tom@check testgit2]$ git init
        Initialized empty Git repository in /home/tom/in/testgit/testgit2/.git/

3. 下传pull

    1. remote报错啦

            [tom@check testgit2]$ git remote add checkgit git@192.168.31.181:/srv/OpenX.git
            [tom@check testgit2]$ git pull checkgit master
            git@192.168.31.181's password:
            fatal: '/srv/OpenX.git' does not appear to be a git repository
            fatal: Could not read from remote repository.

            Please make sure you have the correct access rights
            and the repository exists.
            [tom@check testgit2]$

    2. clone也是会报错哟

            [tom@check testgit2]$ git clone git@192.168.31.181:/srv/OpenX.git
            Cloning into 'OpenX'...
            git@192.168.31.181's password:
            fatal: '/srv/OpenX.git' does not appear to be a git repository
            fatal: Could not read from remote repository.

            Please make sure you have the correct access rights
            and the repository exists.
            [tom@check testgit2]$

## 解决　

查看文件属性

    [tom@check testgit2]$ ll /srv/
    total 0
    drwxr-xr-x 7 git git 111 Jan 19 18:13 conan.git
    drwxr-xr-x 7 git git 111 Jan 11 10:52 crawler.git
    drwxr-xr-x 7 git git 111 Jan  8 16:19 eastmoney.git
    drwx------ 7 tom tom 107 Feb 28 08:59 OpenX.git
    drwxr-xr-x 7 git git 111 Jan 19 11:18 qlw.git
    drwxr-xr-x 7 git git 111 Jan  6 15:03 sample.git
    [tom@check testgit2]$

发现OpenX.git 属性不对.

检查是否安装git,并有git用户。如果没有，则建立git账户和密码
修改 OpenX.git 属性

    [tom@check testgit2]$ sudo chown -R git:git /srv/OpenX.git/
    [tom@check testgit2]$ sudo chmod -R 755 /srv/OpenX.git/
    [tom@check testgit2]$ ll /srv/
    total 0
    drwxr-xr-x 7 git git 111 Jan 19 18:13 conan.git
    drwxr-xr-x 7 git git 111 Jan 11 10:52 crawler.git
    drwxr-xr-x 7 git git 111 Jan  8 16:19 eastmoney.git
    drwxr-xr-x 7 git git 107 Feb 28 08:59 OpenX.git
    drwxr-xr-x 7 git git 111 Jan 19 11:18 qlw.git
    drwxr-xr-x 7 git git 111 Jan  6 15:03 sample.git
    [tom@check testgit2]$

验收

    [tom@check testgit2]$ git remote add checkgit git@192.168.31.181:/srv/OpenX.git
    fatal: remote checkgit already exists.
    [tom@check testgit2]$ git pull checkgit master
    git@192.168.31.181's password:
    remote: Counting objects: 1673, done.
    remote: Compressing objects: 100% (1637/1637), done.
    remote: Total 1673 (delta 928), reused 76 (delta 32)
    Receiving objects: 100% (1673/1673), 6.90 MiB | 7.48 MiB/s, done.
    Resolving deltas: 100% (928/928), done.
    From 192.168.31.181:/srv/OpenX
     * branch            master     -> FETCH_HEAD
    [tom@check testgit2]$ ls
    lib  OpenInfo  OpenQuote
    [tom@check testgit2]$
