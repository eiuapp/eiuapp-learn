---
title: "Qor Admin Test Change Title"
date: 2018-07-03T18:45:58+08:00
draft: false
tags: ["qor"]
categories: ["qor"]
subtitle: "qor/admin 项目的个性化配置(依赖包)"
descriptions: ""
bigimg:
---

qor/admin 项目的个性化配置(依赖包)

这里使用 qor/admin 项目的 test 文件夹中的用例，启动。

## Env

- os: ubuntu16
- go: 1.10.2


## Step

现在我们启动项目后，会先安装依赖，再配置依赖包。

### 安装依赖

查看 test文件夹下的文件，找出依赖包，整理一下，安装

```
go get github.com/qor/media
go get github.com/jinzhu/gorm
go get github.com/qor/admin
go get github.com/qor/qor
```
### 个性化配置

#### 找到`go get`文件

以我的代码习惯，`go get`文件放在了 `$HOME/golang/lib` 中

所以qor/admin项目文件，就在  `$HOME/golang/lib/src/github.com/qor/admin` 中

```
cd ~/golang/lib/src/github.com/qor/admin
```

#### 配置logo

logo 在 `views/assets/images/logo.png` 中，替换就可以了。

#### 配置左下角的 `Powered by`

`Powered by` 在 `views/shared/sidebar.tmpl`

#### 配置标签页面的标题

标签页面的标题 在 `views/layout.tmpl`

## Command

```
ubuntu@VM-0-12-ubuntu:~/golang/lib/src/github.com/qor/admin$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   views/assets/images/logo.png
	modified:   views/layout.tmpl
	modified:   views/shared/sidebar.tmpl

no changes added to commit (use "git add" and/or "git commit -a")
ubuntu@VM-0-12-ubuntu:~/golang/lib/src/github.com/qor/admin$ git diff 
diff --git a/views/assets/images/logo.png b/views/assets/images/logo.png
index 37da102..1201a78 100644
Binary files a/views/assets/images/logo.png and b/views/assets/images/logo.png differ
diff --git a/views/layout.tmpl b/views/layout.tmpl
index 6aa17ef..c68f759 100644
--- a/views/layout.tmpl
+++ b/views/layout.tmpl
@@ -10,7 +10,7 @@
   -->
   <head>
     {{$title := page_title}}
-    <title>{{if $title}}{{$title}} - {{end}}{{if .Admin.SiteName}}{{t .Admin.SiteName}}{{else}}{{t "Qor Admin"}}{{end}}</title>
+    <title>{{if $title}}{{$title}} - {{end}}{{if .Admin.SiteName}}{{t .Admin.SiteName}}{{else}}{{t "<E6><96><87><E5><BC><BA><E7><9A><84><E5><BA><97>"}}{{end}}</title>
     <meta charset="utf-8">
     <meta http-equiv="x-ua-compatible" content="ie=edge">
     <meta name="viewport" content="width=device-width, initial-scale=1">
diff --git a/views/shared/sidebar.tmpl b/views/shared/sidebar.tmpl
index 4a85658..8743614 100644
--- a/views/shared/sidebar.tmpl
+++ b/views/shared/sidebar.tmpl
@@ -22,6 +22,7 @@
     </div>
   </div>
   <div class="sidebar-footer">
-    {{t "qor_admin.layout.powered_by" "Powered by <a href=\"http://getqor.com\" target=\"_blank\">QOR</a>"}}
+    <!-- {{t "qor_admin.layout.powered_by" "Powered by <a href=\"http://getqor.com\" target=\"_blank\">QOR</a>"}} -->
+    {{t "qor_admin.layout.powered_by" "Powered by <a href=\"https://blog.finsoft.info\" target=\"_blank\">blog.finsoft.info</a>"}}
   </div>
 </div>
ubuntu@VM-0-12-ubuntu:~/golang/lib/src/github.com/qor/admin$ 
```



## Ref

- https://github.com/qor/admin