---
title: "Git Faq A"
date: 2018-06-14T11:56:14+08:00
draft: false
tags: ["git"]
categories: ["git"]
subtitle: "报错 `Updates were rejected because the tip of your current branch is behind`"
descriptions: ""
bigimg:
---

git 报错 `Updates were rejected because the tip of your current branch is behind`

## Env
```
➜  public git:(master) git push -u origin master
To bitbucket.org:tomtsang/blog_tomtsang_hugo_html.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@bitbucket.org:tomtsang/blog_tomtsang_hugo_html.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
## Step

查看大部分资料，只有这个有用

http://www.cnblogs.com/xwdreamer/archive/2012/05/29/2523958.html

勾选强制覆盖已有的分支（可能会丢失改动），再点击上传，上传成功。

只有这句是核心，所以，本人就略微想了一下

`git push -u origin master -f`

至此，搞定问题

```
➜  public git:(master) git push -u origin master -f
Counting objects: 1832, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (891/891), done.
Writing objects: 100% (1832/1832), 1.58 MiB | 7.23 MiB/s, done.
Total 1832 (delta 868), reused 723 (delta 283)
remote: Resolving deltas: 100% (868/868), done.
To bitbucket.org:tomtsang/blog_tomtsang_hugo_html.git
 + c0059fb...1eb30ee master -> master (forced update)
Branch 'master' set up to track remote branch 'master' from 'origin'.
➜  public git:(master)
```


## Ref

- https://blog.csdn.net/shiren1118/article/details/7761203