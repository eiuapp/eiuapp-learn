---
title: "Svn Authorization Failed"
date: 2018-06-28T18:10:29+08:00
draft: false
tags: ["svn"]
categories: ["svn"]
subtitle: "如何解决svn Authorization failed错误"
descriptions: ""
bigimg:
---

如何解决svn Authorization failed错误

## Env

- svn: 1.9.7

## Step

出现这种问题肯定是

- SVN服务器
- 清除一下svn的缓存


### SVN服务器

SVN服务器出现了问题，需要修改其三个配置文件： 

1、svnserve.conf: 

```
[general] 
anon-access = read 
auth-access = write 
password-db = passwd 
authz-db = authz
```

2、passwd: 

```
[users] 
admin=123
```
3、authz: 

```
[groups] 
[/] 
admin= rw
```

出现authorization failed异常，一般都是authz文件或者svnserve.conf里，用户组或者用户权限没有配置好，只要设置[/]就可以，代表根目录下所有的资源，如果要限定资源，可以加上 子目录即可。

### 清除一下svn的缓存

右击桌面，找到TortoiseSVN下面的`Settings`，`Saved data`, `Authentication data`点击`clear`

注意：这里不要全部清除，清除你刚才输入的那个svn地址的缓存，然后确定退出。


## Ref

- https://blog.csdn.net/qq_26291823/article/details/70846732
- https://blog.csdn.net/zhouchenxuan/article/details/71249655