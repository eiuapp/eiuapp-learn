---
title: "Nodejs Version"
date: 2018-03-02T15:08:46+08:00
draft: false
tags: ["nodejs", "version"]
categories: ["nodejs"]
subtitle: ""
descriptions: ""
bigimg:
---

nodejs系列之使用n管理nodejs版本

## ref

- http://blog.csdn.net/jiangbo_phd/article/details/51476155

## 主要点

npm -g XXX :安装的XXX软件，在linux下的目录 `/usr/lib/node_modules/`

sudo n :切换node版本

n 切换之后的 node 默认装在 /usr/local/bin/node，你最好用 which node 检查一下当前使用的 node 是否是这个路径下的。

## 问题

* 如果安装过程中因为某原因（主要是网络原因），未安装成功，则删除对应的目录，重新安装就可以了。

    tom@adata:~/Projects/OpenX/lib$ sudo n latest
    [sudo] password for tom:

         install : node-v7.2.0
           mkdir : /usr/local/n/versions/node/7.2.0
           fetch : https://nodejs.org/dist/v7.2.0/node-v7.2.0-linux-x64.tar.gz
    ######################################################                    75.1%
    curl: (56) GnuTLS recv error (-54): Error in the pull function.

    gzip: stdin: unexpected end of file
    tar: Unexpected EOF in archive
    tar: Unexpected EOF in archive
    tar: Error is not recoverable: exiting now
    cp: cannot stat '/usr/local/n/versions/node/7.2.0/lib': No such file or directory
    cp: cannot stat '/usr/local/n/versions/node/7.2.0/include': No such file or directory
    cp: cannot stat '/usr/local/n/versions/node/7.2.0/share': No such file or directory
       installed : v7.2.0

    tom@adata:~/Projects/OpenX/lib$ ll /usr/local/n/versions/node/7.2.0/
    total 12
    drwxr-xr-x 3 root root 4096 11月 23 13:07 ./
    drwxr-xr-x 4 root root 4096 11月 23 13:00 ../
    drwxrwxr-x 2  500  500 4096 11月 23 06:35 bin/
    tom@adata:~/Projects/OpenX/lib$ sudo n latest
    [sudo] password for tom:
    tom@adata:~/Projects/OpenX/lib$ sudo n latest
    tom@adata:~/Projects/OpenX/lib$ sudo rm -rf /usr/local/n/versions/node/7.2.0/
    tom@adata:~/Projects/OpenX/lib$ sudo n latest

         install : node-v7.2.0
           mkdir : /usr/local/n/versions/node/7.2.0
           fetch : https://nodejs.org/dist/v7.2.0/node-v7.2.0-linux-x64.tar.gz
    ######################################################################## 100.0%
       installed : v7.2.0

    tom@adata:~/Projects/OpenX/lib$

* 安装完成后，不要直接看node版本，另开一个terminal，查看。

    tom@adata:~/Projects/OpenX/lib$ node -v
    v6.7.0
    tom@adata:~/Projects/OpenX/lib$

看吧，上面这个明显还是老版本。另开一个terminal，查看就是新版本了。

## 实战

    [jlch@kube-node-15 ~]$ sudo npm i n -g
    [sudo] password for jlch:
    /usr/bin/n -> /usr/lib/node_modules/n/bin/n
    n@2.1.4 /usr/lib/node_modules/n
    [jlch@kube-node-15 ~]$ ll /usr/lib/node_modules/  # 看一下，n是不是真的安装成功
    total 8
    drwxr-xr-x. 3 nobody jlch   66 11月 23 14:08 n
    drwxr-xr-x. 9 root   root 4096 6月   8 16:05 npm
    drwxr-xr-x. 5 nobody jlch 4096 6月   8 18:00 pm2
    [jlch@kube-node-15 ~]$ sudo n --help # 找帮助
      Usage: n [options/env] [COMMAND] [args]

      Environments:
        n [COMMAND] [args]            Uses default env (node)
        n io [COMMAND]                Sets env as io
        n project [COMMAND]           Uses custom env-variables to use non-official sources

      Commands:

        n                              Output versions installed
        n latest                       Install or activate the latest node release
        n -a x86 latest                As above but force 32 bit architecture
        n stable                       Install or activate the latest stable node release
        n lts                          Install or activate the latest LTS node release
        n <version>                    Install node <version>
        n use <version> [args ...]     Execute node <version> with [args ...]
        n bin <version>                Output bin path for <version>
        n rm <version ...>             Remove the given version(s)
        n --latest                     Output the latest node version available
        n --stable                     Output the latest stable node version available
        n --lts                        Output the latest LTS node version available
        n ls                           Output the versions of node available

      (iojs):
        n io latest                    Install or activate the latest iojs release
        n io -a x86 latest             As above but force 32 bit architecture
        n io <version>                 Install iojs <version>
        n io use <version> [args ...]  Execute iojs <version> with [args ...]
        n io bin <version>             Output bin path for <version>
        n io rm <version ...>          Remove the given version(s)
        n io --latest                  Output the latest iojs version available
        n io ls                        Output the versions of iojs available

      Options:

        -V, --version   Output current version of n
        -h, --help      Display help information
        -q, --quiet     Disable curl output (if available)
        -d, --download  Download only
        -a, --arch      Override system architecture

      Aliases:

        which   bin
        use     as
        list    ls
        -       rm

    [jlch@kube-node-15 ~]$ sudo n --version
    2.1.3
    [jlch@kube-node-15 ~]$ node -v
    v4.4.5
    [jlch@kube-node-15 ~]$ sudo n 4.4.6

         install : node-v4.4.6
           mkdir : /usr/local/n/versions/node/4.4.6
           fetch : https://nodejs.org/dist/v4.4.6/node-v4.4.6-linux-x64.tar.gz
    ##################                                                        26.0%

查一下，各自的路径

    tom@adata:~/m6s/blogs/tomtsang$ which node
    /usr/local/bin/node
    tom@adata:~/m6s/blogs/tomtsang$ which n
    /usr/bin/n
    tom@adata:~/m6s/blogs/tomtsang$

切换着玩一下。

    tom@adata:~/Projects/OpenX/lib$ node -v
    v6.9.1
    tom@adata:~/Projects/OpenX/lib$ sudo n
    [sudo] password for tom:
    tom@adata:~/Projects/OpenX/lib$ node -v
    v7.2.0
    tom@adata:~/Projects/OpenX/lib$ sudo n
    tom@adata:~/Projects/OpenX/lib$ node -v
    v6.9.1
    tom@adata:~/Projects/OpenX/lib$
