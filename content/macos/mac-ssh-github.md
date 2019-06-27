---
title: "Mac上SSH-Key对应多个github账号"
date: 2018-04-07T22:30:59+08:00
draft: false
tags: ["mac", "ssh", "github"]
categories: ["github"]
subtitle: ""
descriptions: ""
bigimg:
---

Mac 上SSH-Key对应多个github账号

当然, 也不单是github帐号, gitlab或者其它账号都是可以的哟...

## 前言

因为最近在其他公司帮忙，而其公司用的是他们自己的git服务器，自己本公司又有自己的git服务器，然后自己还用github，造成三个git账号的都要ssh-key，而在网上一搜生成ssh-key的方法都是直接就给你弄全局了，然后肯定又会覆盖原有的ssh-key，所以查了一下关于同机器多账号的ssh-key配置，在此记录一下。

## 操作步骤

如果我们Mac上面已经有了ssh-key再创建ssh-key的话，需要给我们的ssh-key文件取不同的名字，默认是id_rsa，如果不重新起名的话，会把原有的给覆盖掉。

### 1.新建ssh-key&重新命名

```
//切换到ssh目录
cd ~/.ssh
//新建ssh-key
ssh-keygen -t rsa -C  "mywork@email.com"
 //为新建的ssh-key重新命名
Enter file in which to save the key (/Users/bombvote-zql/.ssh/id_rsa):id_ras_bill_github
```
### 2.新ssh-key添加到ssh agent中

因为默认只读取id_rsa，为了让SSH识别新的私钥，需将其添加到SSH agent中：


```
ssh-add ~/.ssh/id_ras_bill_github
```
### 3.配置 将不同账号的工程图服务器与ssh-key关联

```
#thub user(first@email.com)
Host github1
 HostName git.some.com/
 User git
 IdentityFile /Users/bombvote-zql/.ssh/id_rsa

# second user(second@email.com)
 # 建一个github别名，新建的帐号使用这个别名做克隆和更新
Host github2
 HostName github.com
 User git
 IdentityFile /Users/bombvote-zql/.ssh/id_ras_bill_github
```

### 4.在git服务器上添加公钥

```
vim ~/.ssh/id_rsa_bill_github.pub
```
然后将内容复制添加到服务器账号里面
其规则就是：从上至下读取`config`的内容，在每个Host下寻找对应的私钥。这里将GitHub SSH仓库地址中的`git@github.com`替换成新建的Host别名如：github2，那么原地址是：`git@github.com:username/Mywork.git`，替换后应该是：`github2:username/Mywork.git`.

测试一下：
```
ssh -T github2

Hi 0xJoker! You've successfully authenticated, but GitHub does not provide shell
```

链接：https://www.jianshu.com/p/65303f8e5f10

## step

看一下我的配置, 以及一个内部gitlab的仓库, 可能你就明白了...

其实可以理解成 Host(如:`jlch_gitlab_169_root`) 代替 `User@HostName` (如:`git@192.168.31.169`)
整合起来就是: `jlch_gitlab_169_root:root/test.git` 代替 `git@192.168.31.169:root/test.git` 

```
➜  .ssh pwd
/Users/tom/.ssh
➜  .ssh ls
authorized_keys                 id_rsa.pub                      id_rsa_jlch_gitlab_148_root.pub id_rsa_jlch_gitlab_169_root.pub tomMacAir
cdn240                          id_rsa_jlch_gitlab_169_a        id_rsa_jlch_gitlab_a
config                          id_rsa_jlch_gitlab_169_a.pub    id_rsa_jlch_gitlab_a.pub
id_rsa                          id_rsa_jlch_gitlab_148_root     id_rsa_jlch_gitlab_169_root     known_hosts
➜  .ssh cat config
Host jlch_gitlab_148_root
  HostName 192.168.31.148
  User git
  IdentityFile /Users/tom/.ssh/id_rsa_jlch_gitlab_148_root

Host jlch_gitlab_148_tom
  HostName 192.168.31.148
  User git
  IdentityFile /Users/tom/.ssh/id_rsa_jlch_gitlab_a

Host jlch_gitlab_169_root
  HostName 192.168.31.169
  User git
  IdentityFile /Users/tom/.ssh/id_rsa_jlch_gitlab_169_root

Host jlch_gitlab_169_tom
  HostName 192.168.31.169
  User git
  IdentityFile /Users/tom/.ssh/id_rsa_jlch_gitlab_169_a
➜  .ssh cd ~/gitlab-test/169-root-test
➜  169-root-test git:(master) git remote -v
origin	jlch_gitlab_169_root:root/test.git (fetch)
origin	jlch_gitlab_169_root:root/test.git (push)
➜  169-root-test git:(master)
```


## ref

- [Mac 上SSH-Key对应多个git账号](https://www.jianshu.com/p/65303f8e5f10)
