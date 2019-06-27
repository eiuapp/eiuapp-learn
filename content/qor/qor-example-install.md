---
title: "Qor Example Install"
date: 2018-07-02T16:32:56+08:00
draft: false
tags: ["qor","ERP"]
categories: ["ERP"]
subtitle: "qor-example 安装启动"
descriptions: ""
bigimg:
---

qor-example 安装启动

## Env

- os: ubuntu16
- go: 1.10.2

## Step


### 设置 GOPATH

添加 `/home/tom/golang/qor`为GOPATH
```
root@tom-w10vbud16:/home/tom/ztom/bits/pem/aws/aws-account-b/pem# export | grep go
declare -x GOPATH="/home/tom/golang/lib:/home/tom/golang/goc2p:/home/tom/ztom/project/github/ERP/goERP:/home/tom/golang/qor"
declare -x GOROOT="/usr/local/go"
declare -x PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/local/go/bin:/home/tom/golang/lib/bin"
root@tom-w10vbud16:/home/tom/ztom/bits/pem/aws/aws-account-b/pem# 
```

### 安装

```
# Get example app
$ go get -u github.com/qor/qor-example

# Setup database
$ mysql -uroot -p
mysql> CREATE DATABASE IF NOT EXISTS qor_example DEFAULT CHARSET utf8 COLLATE utf8_general_ci;

# Run Application
$ cd $GOPATH/src/github.com/qor/qor-example
$ go run main.go
```

注意，这里不能直接使用 `mysql> CREATE DATABASE qor_example;`因为这样会使用默认字符集latin1，导致中文无法输入，且会报错。

最后一步，报错了。

```
root@tom-w10vbud16:/home/tom/golang/lib/src/github.com/qor/qor-example# go run main.go 
Failed to find configuration config/application.yml, using example file config/application.example.yml
Failed to find configuration config/smtp.yml, using example file config/smtp.example.yml
WARNING: AssetFS is used before overwrite it!
goroutine 1 [running, locked to thread]:
runtime/debug.Stack(0xc4200a6008, 0xc42036fd08, 0x1)
	/usr/local/go/src/runtime/debug/stack.go:24 +0xa7
runtime/debug.PrintStack()
	/usr/local/go/src/runtime/debug/stack.go:16 +0x22
github.com/qor/assetfs.SetAssetFS(0x7f46b008c0d0, 0x140e160)
	/home/tom/golang/lib/src/github.com/qor/assetfs/assetfs.go:33 +0xb0
github.com/qor/qor-example/config/bindatafs.init.0()
	/home/tom/golang/lib/src/github.com/qor/qor-example/config/bindatafs/bindatafs.go:26 +0x5d
panic: Error 1045: Access denied for user 'root'@'localhost' (using password: NO)

goroutine 1 [running]:
github.com/qor/qor-example/config/db.init.0()
	/home/tom/golang/lib/src/github.com/qor/qor-example/config/db/db.go:49 +0x799
exit status 2
root@tom-w10vbud16:/home/tom/golang/lib/src/github.com/qor/qor-example# 
```

这个明显是mysql错误，首先要排除登录问题。

```
root@tom-w10vbud16:/home/tom/golang/lib/src/github.com/qor/qor-example# mysql -uroot  -h localhost -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 11
Server version: 5.7.22-0ubuntu0.16.04.1 (Ubuntu)

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

说明本机 `'root'@'localhost'`用户是可以登录的。
那就是密码配置问题咯。

### 配置mysql

找到配置文件，发现是在 config/database.example.yml 文件中配置的。那生成新配置文件，并添加密码吧。

```
root@tom-w10vbud16:/home/tom/golang/lib/src/github.com/qor/qor-example# cp config/application.example.yml config/database.yml
root@tom-w10vbud16:/home/tom/golang/lib/src/github.com/qor/qor-example# vi config/database.yml 
root@tom-w10vbud16:/home/tom/golang/lib/src/github.com/qor/qor-example# cat config/database.yml 
db:
  adapter: mysql
  name: qor_example
  user: root
  password: root
root@tom-w10vbud16:/home/tom/golang/lib/src/github.com/qor/qor-example# 
```

### gitter

再次 `go run main.go` 报`Failed to find configuration config/application.yml, using example file config/application.example.yml`

然后去到了 qor的 gitter 中去找找相同经历。
果然找到了。

```
nawikart @nawikart 4月 02 21:00
hi guys, anyone can help me about QOR Quick Started please..
I already do these:

Get example app
$ go get -u github.com/qor/qor-example

Setup database
$ mysql -uroot -p
mysql> CREATE DATABASE qor_example;

Run Application
$ cd $GOPATH/src/github.com/qor/qor-example
$ go run main.go

but got result:

Failed to find configuration config/application.yml, using example file config/application.example.yml

is anyone would like to explain what happened please.....

Thanateros @thanateros 4月 03 03:14
@nawikart you also need to have NodeJS installed, then install 'gulp' globally with npm and do npm install (in application root directory).

Then pull go dependencies (go get -u ./... from application root directory) -- this part may take a while and gives no feedback until error or done, so you can throw a -v switch in there (go get -u -v ./... if you want feedback while it works).

Do not forget to edit the database connection information (edit qor-example/config/config.go).

Then run with go run main.go or go run -v main.go if you want some verbose output.

I recall having to piece all of this together myself (except for the go get ./... part, that is actually there if you click the wiki link for the qor-example repo).

You are almost there my fellow gopher.
```

总之，就是运行下面的命令

```
cnpm i -g gulp
cd $GOPATH/src/github/qor/qor-example
go get -u ./...
go run -v main.go
```

最后，我的效果如下

```
root@tom-w10vbud16:/home/tom/golang/lib/src/github.com/qor/qor-example# go run main.go 
Failed to find configuration config/application.yml, using example file config/application.example.yml
Failed to find configuration config/smtp.yml, using example file config/smtp.example.yml
WARNING: AssetFS is used before overwrite it!
goroutine 1 [running, locked to thread]:
runtime/debug.Stack(0xc42000e018, 0xc42038fd08, 0x1)
	/usr/local/go/src/runtime/debug/stack.go:24 +0xa7
runtime/debug.PrintStack()
	/usr/local/go/src/runtime/debug/stack.go:16 +0x22
github.com/qor/assetfs.SetAssetFS(0x7f3bb5e54000, 0x140e160)
	/home/tom/golang/lib/src/github.com/qor/assetfs/assetfs.go:33 +0xb0
github.com/qor/qor-example/config/bindatafs.init.0()
	/home/tom/golang/lib/src/github.com/qor/qor-example/config/bindatafs/bindatafs.go:26 +0x5d
Failed to create unique index for translations key & locale, got: Error 1170: BLOB/TEXT column 'key' used in key specification without a key length

(Error 1170: BLOB/TEXT column 'key' used in key specification without a key length) 
[2018-07-02 16:41:27]  
Enterprise features not enabled...
Listening on: 7000
```

## Ref

- https://github.com/qor/qor-example
- https://gitter.im/qor/qor