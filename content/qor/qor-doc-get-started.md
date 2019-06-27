---
title: "Qor Doc Get Started"
date: 2018-07-02T18:54:49+08:00
draft: false
tags: ["qor", "learn"]
categories: ["qor"]
subtitle: "Qor Doc中的Get Started"
descriptions: ""
bigimg:
---

qor step by step

Qor Doc中的Get Started


## Env

- os: ubuntu16
- go: 1.10.2

## Step

```
root@tom-w10vbud16:/home/tom/test# cd /home/tom/golang/
root@tom-w10vbud16:/home/tom/golang# mkdir qor/src -p
root@tom-w10vbud16:/home/tom/golang# cd qor/src/
root@tom-w10vbud16:/home/tom/golang/qor/src# git clone https://github.com/qor/qor
root@tom-w10vbud16:/home/tom/golang/qor/src# ls qor/
bower.json  config.go  context.go  errors.go  gulpfile.js  LICENSE.txt  package.json  README.md  resource  test  test_all.sh  update_all_qor_repos.sh  utils  yarn.lock
```

### get_started

https://doc.getqor.com/get_started.html

#### 配置 GOPATH

把 `/home/tom/golang/qor` 加入 `GOPATH`
```
root@tom-w10vbud16:/home/tom/golang/qor/src# vi ~/.bashrc 
root@tom-w10vbud16:/home/tom/golang/qor/src# source ~/.bashrc 
root@tom-w10vbud16:/home/tom/golang/qor/src# export | grep go
declare -x GOPATH="/home/tom/golang/lib:/home/tom/golang/goc2p:/home/tom/ztom/project/github/ERP/goERP:/home/tom/golang/qor"
declare -x GOROOT="/usr/local/go"
declare -x OLDPWD="/home/tom/golang"
declare -x PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/local/go/bin:/home/tom/golang/lib/bin:/usr/local/go/bin:/home/tom/golang/lib/bin"
declare -x PWD="/home/tom/golang/qor/src"
```

```
root@tom-w10vbud16:/home/tom/golang/qor/src# vi main.go
root@tom-w10vbud16:/home/tom/golang/qor/src# chmod +x main.go 
```

#### 把 main.go 中涉及的包，安装一下

```
root@tom-w10vbud16:/home/tom/golang/qor/src# ls
main.go  qor
root@tom-w10vbud16:/home/tom/golang/qor/src# go get github.com/qor/qor
root@tom-w10vbud16:/home/tom/golang/qor/src# go get github.com/qor/admin
root@tom-w10vbud16:/home/tom/golang/qor/src# go get github.com/jinzhu/gorm
root@tom-w10vbud16:/home/tom/golang/qor/src# go get github.com/jinzhu/gorm/dialects/sqlite
```

#### run

```
root@tom-w10vbud16:/home/tom/golang/qor/src# ls
main.go  qor
root@tom-w10vbud16:/home/tom/golang/qor/src# go run main.go 
# command-line-arguments
./main.go:6:3: imported and not used: "github.com/qor/qor"
root@tom-w10vbud16:/home/tom/golang/qor/src# 
```

好像说`github.com/qor/qor`这个包没有用到，那先注释掉吧

```
root@tom-w10vbud16:/home/tom/golang/qor/src# pwd
/home/tom/golang/qor/src
root@tom-w10vbud16:/home/tom/golang/qor/src# ls
main.go  qor
root@tom-w10vbud16:/home/tom/golang/qor/src# vi main.go 
root@tom-w10vbud16:/home/tom/golang/qor/src# go run main.go 
Listening on: 9000
2018/07/02 14:43:21 Finish [GET] /admin Took 4.04ms
2018/07/02 14:48:33 Finish [GET] /admin Took 2.25ms
2018/07/02 14:48:35 Finish [GET] /admin/users Took 6.25ms
2018/07/02 14:48:36 Finish [GET] /admin/products Took 7.03ms
2018/07/02 14:50:50 Finish [GET] /admin/products Took 5.62ms
2018/07/02 14:50:54 Finish [GET] /admin Took 2.21ms
2018/07/02 16:57:57 Finish [GET] /admin/users Took 15.72ms
2018/07/02 16:57:58 Finish [GET] /admin/products Took 4.41ms
2018/07/02 16:57:59 Finish [GET] /admin/users Took 12.21ms
2018/07/02 16:58:00 Finish [GET] /admin/products Took 4.65ms
^Csignal: interrupt
root@tom-w10vbud16:/home/tom/golang/qor/src# 
```

这个时候，会多出一个`demo.db`文件
```
root@tom-w10vbud16:/home/tom/golang/qor/src# ls
demo.db  main.go  qor
```

成功了。虽然这个demo没有什么东西。



## Ref

- https://doc.getqor.com/