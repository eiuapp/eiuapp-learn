---
title: "Qor Admin Test"
date: 2018-07-03T18:16:42+08:00
draft: false
tags: ["qor"]
categories: ["qor"]
subtitle: "qor/admin 项目的 test 文件夹中的用例"
descriptions: ""
bigimg:
---

qor/admin 项目的 test 文件夹中的用例

## Env

- os: ubuntu16
- go: 1.10.2


## Step

### 安装依赖

查看 test文件夹下的文件，找出依赖包，整理一下，安装

```
go get github.com/qor/media
go get github.com/jinzhu/gorm
go get github.com/qor/admin
go get github.com/qor/qor
```

### 配置mysql

```
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ go run main.go
Listening on: 3000
panic: Error 1045: Access denied for user 'qor'@'localhost' (using password: YES)

goroutine 1 [running]:
github.com/qor/qor/test/utils.TestDB(0x30)
        /home/tom/golang/lib/src/github.com/qor/qor/test/utils/test_db.go:36 +0x585
github.com/qor/admin/tests/dummy.NewDummyAdmin(0xc42031bf1f, 0x1, 0x1, 0xa791a0)
        /home/tom/golang/lib/src/github.com/qor/admin/tests/dummy/admin.go:15 +0x37
main.main()
        /home/tom/golang/qor/src/qor/tom/admin/admin/tests/main.go:22 +0x187
exit status 2
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ 
```

有报错了。报错，说明了，在我们的 `go get github.com/qor/qor` 中的 `utils.TestDB` 出错了，文件夹是在 `/home/tom/golang/lib/src/github.com/qor/qor/test/utils/test_db.go`.
那容易了，我们直接去grep一下。

```
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ grep localhost -rn ./
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ grep localhost -rn ./../../
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ grep localhost -rn ./../../../../../../../lib/src/github.com/qor/qor/test/utils/
./../../../../../../../lib/src/github.com/qor/qor/test/utils/test_db.go:27:             db, err = gorm.Open("postgres", fmt.Sprintf("postgres://%s:%s@localhost/%s?sslmode=disable", dbuser, dbpwd, dbname))
./../../../../../../../lib/src/github.com/qor/qor/test/utils/test_db.go:29:             // CREATE USER 'qor'@'localhost' IDENTIFIED BY 'qor';
./../../../../../../../lib/src/github.com/qor/qor/test/utils/test_db.go:31:             // GRANT ALL ON qor_test.* TO 'qor'@'localhost';
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ 
```

果然是有的。
提示说了，`// CREATE USER 'qor'@'localhost' IDENTIFIED BY 'qor';`, `// GRANT ALL ON qor_test.* TO 'qor'@'localhost';`, 那我们先把这些创建好吧。

```
mysql> CREATE USER 'qor'@'localhost' IDENTIFIED BY 'qor';
mysql> GRANT ALL ON qor_test.* TO 'qor'@'localhost';
```

再启动

```
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ ls
dummy  logo.png  main.go  qor.png  views
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ go run main.go
Listening on: 3000
panic: Error 1049: Unknown database 'qor_test'

goroutine 1 [running]:
github.com/qor/qor/test/utils.TestDB(0x30)
        /home/tom/golang/lib/src/github.com/qor/qor/test/utils/test_db.go:36 +0x585
github.com/qor/admin/tests/dummy.NewDummyAdmin(0xc42033ff1f, 0x1, 0x1, 0xa791a0)
        /home/tom/golang/lib/src/github.com/qor/admin/tests/dummy/admin.go:15 +0x37
main.main()
        /home/tom/golang/qor/src/qor/tom/admin/admin/tests/main.go:22 +0x187
exit status 2
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$
```

好了，现在是另一个报错了，`Unknown database 'qor_test'`。那就去创建这个database吧。

```
mysql> CREATE DATABASE IF NOT EXISTS qor_test DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

注意，这里不能直接使用 `mysql> CREATE DATABASE qor_test;`因为这样会使用默认字符集latin1，导致中文无法输入，且会报错。

再启动

```
tom@tom-w10vbud16:~/golang/qor/src/qor/tom/admin/admin/tests$ go run main.go
Listening on: 3000
2018/07/03 16:07:25 Finish [GET] /admin Took 18.91ms
```

成功了！

## Ref

- https://github.com/qor/admin

