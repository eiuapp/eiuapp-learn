---
title: "Git常用命令与GitHub使用技巧技巧整理"
description: "Git常用命令与GitHub使用技巧日常使用命令整理"
date: "2018-01-31T15:55:36+08:00"
draft: false
categories: ["github"]
tags: ["github","git"]
---

**1. GitHub中同步远程分支**

查看本地已有分支

```
git remote -v
```

增加远程分支

```
git remote add upstream https://github.com/k8smeetup/kubernetes.github.io.git
git fetch upstream
git checkout master
git merge upstream/master
```
**2. 更新Git代码并对比**

```
git remote -v
git fetch origin master
git log -p master.. origin/master
git merge origin/master
```

**3. 删除远程分支**

```
git push origin --delete <branchName>
git push origin --delete tag <tagName>
```

**4. 删除所有历史记录**

Checkout

```
git checkout --orphan latest_branch
```

Add all the files

```
   git add -A
```
Commit the changes

```
   git commit -am "commit message"
```

Delete the branch

```
   git branch -D master
```

Rename the current branch to master

```
   git branch -m master
```
Finally, force update your repository

```
   git push -f origin master
```

#### [同一台电脑设置多个公钥与不同GITHUB帐号交互](https://jingyan.baidu.com/article/219f4bf7f6f8e1de442d3829.html)

- [Mac 上SSH-Key对应多个git账号](https://www.jianshu.com/p/65303f8e5f10)

## 致谢

- [yanchengyc](http://blog.csdn.net/yc1022/article/details/56487680)
- [Git常用命令与GitHub使用技巧技巧整理](https://jimmysong.io/posts/github-tips/)