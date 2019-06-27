---
title: "Qor Admin Test Change Sidebar"
date: 2018-07-03T19:02:41+08:00
draft: false
tags: ["qor"]
categories: ["qor"]
subtitle: "qor/admin 项目的个性化配置(sidebar)"
descriptions: ""
bigimg:
---

qor/admin 项目的个性化配置(sidebar)

这里使用 qor/admin 项目的 test 文件夹中的用例，启动。

## Env

- os: ubuntu16
- go: 1.10.2


## Step

总共修改3个文件

- dummy/admin.go
- dummy/models.go
- main.go

```
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   dummy/admin.go
        modified:   dummy/models.go
        modified:   main.go

no changes added to commit (use "git add" and/or "git commit -a")
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ 
```

### 修改 main.go

- 导入本地的 dummy 包
- 路由路径从 `admin` 到 `wq`

```
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ git diff main.go
diff --git a/tests/main.go b/tests/main.go
index 3328196..bb38c65 100644
--- a/tests/main.go
+++ b/tests/main.go
@@ -5,7 +5,8 @@ import (
        "net/http"
        "os"

-       "github.com/qor/admin/tests/dummy"
+       // "github.com/qor/admin/tests/dummy"
+       "./dummy"
        "github.com/qor/qor/utils"
 )

@@ -19,6 +20,6 @@ func main() {

        mux := http.NewServeMux()
        mux.Handle("/system/", utils.FileServer(http.Dir("public")))
-       dummy.NewDummyAdmin(true).MountTo("/admin", mux)
+       dummy.NewDummyAdmin(true).MountTo("/wq", mux)
        http.ListenAndServe(fmt.Sprintf(":%s", port), mux)
 }
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ 

### 修改 dummy/models.go

`dummy/models.go` 对应的是 mysql 中的表与字段

加减字段

```
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ git diff dummy/models.go
diff --git a/tests/dummy/models.go b/tests/dummy/models.go
index 426eb24..55cf59a 100644
--- a/tests/dummy/models.go
+++ b/tests/dummy/models.go
@@ -16,6 +16,7 @@ type CreditCard struct {
 type Company struct {
        gorm.Model
        Name string
+       Annotation string
 }

 type Address struct {
@@ -27,7 +28,12 @@ type Address struct {

 type Language struct {
        gorm.Model
-       Name string
+       Name                  string
+       Materials                       string
+       Sizes                           string
+       Price                 float32
+       Number                                  uint
+       Description           string
 }

 type User struct {
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ 
```

### 修改 dummy/admin.go

`dummy/admin.go` 中的 `Admin.AddResource` 代表了 sidebar 的内容

导入本地的 `admin`包。这一步，好像也不需要。哈哈！

```
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ git diff  dummy/admin.go
diff --git a/tests/dummy/admin.go b/tests/dummy/admin.go
index ca39a7c..11ab1ee 100644
--- a/tests/dummy/admin.go
+++ b/tests/dummy/admin.go
@@ -1,9 +1,10 @@
 package dummy

 import (
-       "fmt"
+       // "fmt" // 去除 users 时要注释

-       "github.com/qor/admin"
+       // "github.com/qor/admin"
+       "../../../admin"
        "github.com/qor/media"
        "github.com/qor/qor"
        "github.com/qor/qor/test/utils"
@@ -14,6 +15,7 @@ func NewDummyAdmin(keepData ...bool) *admin.Admin {
        var (
                db     = utils.TestDB()
                models = []interface{}{&User{}, &CreditCard{}, &Address{}, &Language{}, &Profile{}, &Phone{}, &Company{}}
+               // Admin  = admin.New(&qor.Config{DB: db, SiteName: "文强的店"})
                Admin  = admin.New(&qor.Config{DB: db})
        )

@@ -26,8 +28,9 @@ func NewDummyAdmin(keepData ...bool) *admin.Admin {
                db.AutoMigrate(value)
        }

-       Admin.AddResource(&Company{})
-       Admin.AddResource(&Language{}, &admin.Config{Name: "语种 & 语言", Priority: -1})
+       // Admin.AddResource(&Company{})
+       Admin.AddResource(&Language{}, &admin.Config{Name: "材料", Priority: -1})
+       /* // 去除 users 时要注释
        user := Admin.AddResource(&User{})
        user.Meta(&admin.Meta{
                Name: "CreditCard",
@@ -45,6 +48,7 @@ func NewDummyAdmin(keepData ...bool) *admin.Admin {
                        return
                },
        })
+       */

        return Admin
 }
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$
```


## Command

命令汇总

```
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   dummy/admin.go
        modified:   dummy/models.go
        modified:   main.go

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        main

no changes added to commit (use "git add" and/or "git commit -a")
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ git diff
diff --git a/tests/dummy/admin.go b/tests/dummy/admin.go
index ca39a7c..11ab1ee 100644
--- a/tests/dummy/admin.go
+++ b/tests/dummy/admin.go
@@ -1,9 +1,10 @@
 package dummy

 import (
-       "fmt"
+       // "fmt" // 去除 users 时要注释

-       "github.com/qor/admin"
+       // "github.com/qor/admin"
+       "../../../admin"
        "github.com/qor/media"
        "github.com/qor/qor"
        "github.com/qor/qor/test/utils"
@@ -14,6 +15,7 @@ func NewDummyAdmin(keepData ...bool) *admin.Admin {
        var (
                db     = utils.TestDB()
                models = []interface{}{&User{}, &CreditCard{}, &Address{}, &Language{}, &Profile{}, &Phone{}, &Company{}}
+               // Admin  = admin.New(&qor.Config{DB: db, SiteName: "文强的店"})
                Admin  = admin.New(&qor.Config{DB: db})
        )

@@ -26,8 +28,9 @@ func NewDummyAdmin(keepData ...bool) *admin.Admin {
                db.AutoMigrate(value)
        }

-       Admin.AddResource(&Company{})
-       Admin.AddResource(&Language{}, &admin.Config{Name: "语种 & 语言", Priority: -1})
+       // Admin.AddResource(&Company{})
+       Admin.AddResource(&Language{}, &admin.Config{Name: "材料", Priority: -1})
+       /* // 去除 users 时要注释
        user := Admin.AddResource(&User{})
        user.Meta(&admin.Meta{
                Name: "CreditCard",
@@ -45,6 +48,7 @@ func NewDummyAdmin(keepData ...bool) *admin.Admin {
                        return
                },
        })
+       */

        return Admin
 }
diff --git a/tests/dummy/models.go b/tests/dummy/models.go
index 426eb24..55cf59a 100644
--- a/tests/dummy/models.go
+++ b/tests/dummy/models.go
@@ -16,6 +16,7 @@ type CreditCard struct {
 type Company struct {
        gorm.Model
        Name string
+       Annotation string
 }

 type Address struct {
@@ -27,7 +28,12 @@ type Address struct {

 type Language struct {
        gorm.Model
-       Name string
+       Name                  string
+       Materials                       string
+       Sizes                           string
+       Price                 float32
+       Number                                  uint
+       Description           string
 }

 type User struct {
diff --git a/tests/main.go b/tests/main.go
index 3328196..bb38c65 100644
--- a/tests/main.go
+++ b/tests/main.go
@@ -5,7 +5,8 @@ import (
        "net/http"
        "os"

-       "github.com/qor/admin/tests/dummy"
+       // "github.com/qor/admin/tests/dummy"
+       "./dummy"
        "github.com/qor/qor/utils"
 )

@@ -19,6 +20,6 @@ func main() {

        mux := http.NewServeMux()
        mux.Handle("/system/", utils.FileServer(http.Dir("public")))
-       dummy.NewDummyAdmin(true).MountTo("/admin", mux)
+       dummy.NewDummyAdmin(true).MountTo("/wq", mux)
        http.ListenAndServe(fmt.Sprintf(":%s", port), mux)
 }
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$
```


## Ref

- https://github.com/qor/admin
