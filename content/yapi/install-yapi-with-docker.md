yapi通过docker安装

https://github.com/Ryan-Miao/docker-yapi 学习笔记

## env

- os: ubuntu
- docker: 18.09.5

## step

### 直接运行 index.sh

```bash
vagrant@ubuntu-xenial:~/yapi$ cat index.sh
#!/bin/bash

git clone https://github.com/Ryan-Miao/docker-yapi.git

cd docker-yapi
bash build.sh 1.5.10
bash start.sh  init-network
bash start.sh start-mongo
bash start.sh init-mongo
bash start.sh init-yapi
bash start.sh logs-yapi
vagrant@ubuntu-xenial:~/yapi$ bash index.sh 
...
...
init mongodb account admin and yapi
MongoDB shell version v4.0.9
connecting to: mongodb://127.0.0.1:27017/admin?gssapiServiceName=mongodb
2019-04-24T03:22:21.172+0000 E QUERY    [js] Error: couldn't connect to server 127.0.0.1:27017, connection attempt failed: SocketException: Error connecting to 127.0.0.1:27017 :: caused by :: Connection refused :
connect@src/mongo/shell/mongo.js:343:13
@(connect):2:6
exception: connect failed
inti mongodb done
init yapi db and start yapi server
5eaca2adb290fb948f7c2c1536cf1d209f92076376359221c555d39b5d6a7f6f
init yapi done
vagrant@ubuntu-xenial:~/yapi$
```

出错了。

此时 `curl http://localhost:3001` 会出现 `curl: (56) Recv failure: Connection reset by peer` 这样的错误。



### (可跳过)bash index.sh 日志 ###

```bash
vagrant@ubuntu-xenial:~/yapi$ ./index.sh
Cloning into 'docker-yapi'...
remote: Enumerating objects: 43, done.
remote: Counting objects: 100% (43/43), done.
remote: Compressing objects: 100% (30/30), done.
remote: Total 43 (delta 20), reused 35 (delta 12), pack-reused 0
Unpacking objects: 100% (43/43), done.
Checking connectivity... done.
usage:  sh build.sh  <version>
yapi的版本：  https://github.com/YMFE/yapi/releases
我们将从这里下载：  http://registry.npm.taobao.org/yapi-vendor/download/yapi-vendor-$1.tgz
将下载版本1.5.10
 download new package (version 1.5.10)
--2019-04-24 03:20:11--  http://registry.npm.taobao.org/yapi-vendor/download/yapi-vendor-1.5.10.tgz
Resolving registry.npm.taobao.org (registry.npm.taobao.org)... 114.55.80.225
Connecting to registry.npm.taobao.org (registry.npm.taobao.org)|114.55.80.225|:80... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://cdn.npm.taobao.org/yapi-vendor/-/yapi-vendor-1.5.10.tgz [following]
--2019-04-24 03:20:11--  https://cdn.npm.taobao.org/yapi-vendor/-/yapi-vendor-1.5.10.tgz
Resolving cdn.npm.taobao.org (cdn.npm.taobao.org)... 183.60.159.232, 183.60.159.230, 183.60.159.228, ...
Connecting to cdn.npm.taobao.org (cdn.npm.taobao.org)|183.60.159.232|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 8395015 (8.0M) [application/octet-stream]
Saving to: ‘yapi.tgz’

yapi.tgz                                                    100%[=========================================================================================================================================>]   8.01M  1.82MB/s    in 4.4s

2019-04-24 03:20:18 (1.82 MB/s) - ‘yapi.tgz’ saved [8395015/8395015]

 build new image
Sending build context to Docker daemon  8.525MB
Step 1/16 : FROM node:11-alpine as builder
11-alpine: Pulling from library/node
bdf0201b3a05: Pull complete
f82315922d4f: Pull complete
e70c14082adc: Pull complete
Digest: sha256:2278992c11ebfba68ca40a56ac77de59f5669b9ce1bb479f89840c95ac1adae7
Status: Downloaded newer image for node:11-alpine
 ---> cd4fae427afc
Step 2/16 : COPY repositories /etc/apk/repositories
 ---> 53171340fe01
Step 3/16 : RUN apk update && apk add --no-cache  git python make openssl tar gcc
 ---> Running in 0cf1f76bfcda
fetch https://mirrors.aliyun.com/alpine/v3.6/main/x86_64/APKINDEX.tar.gz
fetch https://mirrors.aliyun.com/alpine/v3.6/community/x86_64/APKINDEX.tar.gz
v3.6.5-23-g47b45e6408 [https://mirrors.aliyun.com/alpine/v3.6/main/]
v3.6.5-18-gfdfe1f6192 [https://mirrors.aliyun.com/alpine/v3.6/community/]
OK: 8455 distinct packages available
fetch https://mirrors.aliyun.com/alpine/v3.6/main/x86_64/APKINDEX.tar.gz
fetch https://mirrors.aliyun.com/alpine/v3.6/community/x86_64/APKINDEX.tar.gz
(1/32) Installing binutils-libs (2.30-r1)
(2/32) Installing binutils (2.30-r1)
(3/32) Installing gmp (6.1.2-r0)
(4/32) Installing isl (0.17.1-r0)
(5/32) Installing libgomp (6.3.0-r4)
(6/32) Installing libatomic (6.3.0-r4)
(7/32) Installing pkgconf (1.3.7-r0)
(8/32) Installing mpfr3 (3.1.5-r0)
(9/32) Installing mpc1 (1.0.3-r0)
(10/32) Installing gcc (6.3.0-r4)
(11/32) Installing libressl2.5-libcrypto (2.5.5-r2)
(12/32) Installing ca-certificates (20161130-r2)
(13/32) Installing libssh2 (1.8.2-r0)
(14/32) Installing libressl2.5-libssl (2.5.5-r2)
(15/32) Installing libcurl (7.61.1-r2)
(16/32) Installing expat (2.2.0-r1)
(17/32) Installing pcre (8.41-r0)
(18/32) Installing git (2.13.7-r2)
(19/32) Installing make (4.2.1-r0)
(20/32) Installing libcrypto1.0 (1.0.2r-r0)
(21/32) Installing libssl1.0 (1.0.2r-r0)
(22/32) Installing openssl (1.0.2r-r0)
(23/32) Installing libbz2 (1.0.6-r5)
(24/32) Installing libffi (3.2.1-r3)
(25/32) Installing gdbm (1.12-r0)
(26/32) Installing ncurses-terminfo-base (6.0_p20171125-r1)
(27/32) Installing ncurses-terminfo (6.0_p20171125-r1)
(28/32) Installing ncurses-libs (6.0_p20171125-r1)
(29/32) Installing readline (6.3.008-r5)
(30/32) Installing sqlite-libs (3.25.3-r0)
(31/32) Installing python2 (2.7.15-r0)
(32/32) Installing tar (1.32-r0)
Executing busybox-1.29.3-r10.trigger
Executing ca-certificates-20161130-r2.trigger
OK: 164 MiB in 48 packages
Removing intermediate container 0cf1f76bfcda
 ---> d0aaaf657609
Step 4/16 : ADD yapi.tgz /home/
 ---> e90dafc00a41
Step 5/16 : RUN mkdir /api && mv /home/package /api/vendors
 ---> Running in 666e0e900ac9
Removing intermediate container 666e0e900ac9
 ---> c77ac514594b
Step 6/16 : RUN cd /api/vendors &&     npm install --production --registry https://registry.npm.taobao.org
 ---> Running in 470f82e165c5
 Step 6/16 : RUN cd /api/vendors &&     npm install --production --registry https://registry.npm.taobao.org
 ---> Running in 470f82e165c5
npm WARN deprecated babel@6.23.0: In 6.x, the babel package has been deprecated in favor of babel-cli. Check https://opencollective.com/babel to support the Babel maintainers
npm WARN deprecated babel-preset-es2015@6.24.1: ????  Thanks for using Babel: we recommend using babel-preset-env now: please read babeljs.io/env to update!
npm WARN deprecated validate-commit-msg@2.14.0: Check out CommitLint which provides the same functionality with a more user-focused experience.
npm WARN deprecated core-js@2.5.7: core-js@<2.6.5 is no longer maintained. Please, upgrade to core-js@3 or at least to actual version of core-js@2.
npm WARN deprecated fsevents@1.1.2: Way too old
npm WARN deprecated babel-preset-es2017@6.24.1: ????  Thanks for using Babel: we recommend using babel-preset-env now: please read babeljs.io/env to update!
npm WARN deprecated joi@6.10.1: This version has been deprecated in accordance with the hapi support policy (hapi.im/support). Please upgrade to the latest version to get the best features, bug fixes, and security patches. If you are unable to upgrade at this time, paid support is available for older versions (hapi.im/commercial).
npm WARN deprecated hawk@3.1.3: This version has been deprecated. Please upgrade to the latest version to get the best features, bug fixes, and security patches.
npm WARN deprecated browserslist@2.11.3: Browserslist 2 could fail on reading Browserslist >3.0 config used in other tools.
npm WARN deprecated sw-precache@5.2.1: Please migrate to Workbox: https://developers.google.com/web/tools/workbox/guides/migrations/migrate-from-sw
npm WARN deprecated ejs@2.3.4: Critical security bugs fixed in 2.5.5
npm WARN deprecated topo@1.1.0: This version has been deprecated in accordance with the hapi support policy (hapi.im/support). Please upgrade to the latest version to get the best features, bug fixes, and security patches. If you are unable to upgrade at this time, paid support is available for older versions (hapi.im/commercial).
npm WARN deprecated hoek@2.16.3: This version has been deprecated in accordance with the hapi support policy (hapi.im/support). Please upgrade to the latest version to get the best features, bug fixes, and security patches. If you are unable to upgrade at this time, paid support is available for older versions (hapi.im/commercial).
npm WARN deprecated boom@2.10.1: This version has been deprecated in accordance with the hapi support policy (hapi.im/support). Please upgrade to the latest version to get the best features, bug fixes, and security patches. If you are unable to upgrade at this time, paid support is available for older versions (hapi.im/commercial).
npm WARN deprecated sntp@1.0.9: This module moved to @hapi/sntp. Please make sure to switch over as this distribution is no longer supported and may contain bugs and critical security issues.
npm WARN deprecated cryptiles@2.0.5: This version has been deprecated in accordance with the hapi support policy (hapi.im/support). Please upgrade to the latest version to get the best features, bug fixes, and security patches. If you are unable to upgrade at this time, paid support is available for older versions (hapi.im/commercial).
npm WARN deprecated core-js@1.2.7: core-js@<2.6.5 is no longer maintained. Please, upgrade to core-js@3 or at least to actual version of core-js@2.
npm WARN deprecated browserslist@1.7.7: Browserslist 2 could fail on reading Browserslist >3.0 config used in other tools.
npm WARN deprecated circular-json@0.3.3: CircularJSON is in maintenance only, flatted is its successor.
npm WARN deprecated sw-toolbox@3.6.0: Please migrate to Workbox: https://developers.google.com/web/tools/workbox/guides/migrations/migrate-from-sw
npm WARN deprecated nomnom@1.5.2: Package no longer supported. Contact support@npmjs.com for more info.

> dtrace-provider@0.8.7 install /api/vendors/node_modules/dtrace-provider
> node-gyp rebuild || node suppress-error.js

make: Entering directory '/api/vendors/node_modules/dtrace-provider/build'
  TOUCH Release/obj.target/DTraceProviderStub.stamp
make: Leaving directory '/api/vendors/node_modules/dtrace-provider/build'

> jsonpath@1.0.1 postinstall /api/vendors/node_modules/jsonpath
> node lib/aesprim.js > generated/aesprim-browser.js

npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN mongoose-auto-increment@5.0.1 requires a peer of mongoose@^4.1.12 but none is installed. You must install peer dependencies yourself.
npm WARN sass-loader@7.1.0 requires a peer of webpack@^3.0.0 || ^4.0.0 but none is installed. You must install peer dependencies yourself.
npm WARN react-slick@0.15.4 requires a peer of react@^0.14.0 || ^15.0.1 but none is installed. You must install peer dependencies yourself.
npm WARN react-slick@0.15.4 requires a peer of react-dom@^0.14.0 || ^15.0.1 but none is installed. You must install peer dependencies yourself.
npm WARN slick-carousel@1.8.1 requires a peer of jquery@>=1.8.0 but none is installed. You must install peer dependencies yourself.

added 353 packages from 343 contributors and audited 41745 packages in 38.91s
found 117 vulnerabilities (59 low, 34 moderate, 24 high)
  run `npm audit fix` to fix them, or `npm audit` for details
Removing intermediate container 470f82e165c5
 ---> e1de184ab7a0
Step 7/16 : FROM node:11-alpine
 ---> cd4fae427afc
Step 8/16 : MAINTAINER Ryan Miao
 ---> Running in b304c7d45f2c
Removing intermediate container b304c7d45f2c
 ---> bd75d12b26ee
Step 9/16 : ENV TZ="Asia/Shanghai" HOME="/"
 ---> Running in e504582ebe19
Removing intermediate container e504582ebe19
 ---> 583777d1bcd7
Step 10/16 : WORKDIR ${HOME}
 ---> Running in f9212737f022
Removing intermediate container f9212737f022
 ---> 16a0a4f59e15
Step 11/16 : COPY --from=builder /api/vendors /api/vendors
 ---> 3da85b57c8e0
Step 12/16 : COPY config.json /api/
 ---> b71ac66ce6f9
Step 13/16 : EXPOSE 3001
 ---> Running in 21dabe247a61
Removing intermediate container 21dabe247a61
 ---> aa4a013e4b26
Step 14/16 : COPY docker-entrypoint.sh /api/
 ---> 73654cc8b435
Step 15/16 : RUN chmod 755 /api/docker-entrypoint.sh
 ---> Running in d0a2f8e50ca9
Removing intermediate container d0a2f8e50ca9
 ---> 92da279fbb41
 
 ---> 92da279fbb41
Step 16/16 : ENTRYPOINT ["/api/docker-entrypoint.sh"]
 ---> Running in 82902ed862cf
Removing intermediate container 82902ed862cf
 ---> 48a4e40ff4bb
Successfully built 48a4e40ff4bb
Successfully tagged yapi:latest
Error: No such network: tools-net
c417c9379d767c4e8c03c5fc48aae07724f9c2dbf94867bb47ed09c6ffd2d7ff
Error response from daemon: Cannot kill container: mongod: No such container: mongod
Error: No such container: mongod
Unable to find image 'mongo:4' locally
4: Pulling from library/mongo
34667c7e4631: Pull complete
d18d76a881a4: Pull complete
119c7358fbfc: Pull complete
2aaf13f3eff0: Pull complete
f7833eaffdda: Pull complete
8287cb5b9daf: Pull complete
3f92d9c8d3be: Pull complete
c8a99ed04127: Pull complete
7c26ac89d736: Pull complete
f6f01a3e0a56: Pull complete
ff5825a76fb6: Pull complete
0af8892a044b: Pull complete
33eada78dbf2: Pull complete
Digest: sha256:68209a50d83c8e37f3f2dca6d2a69b1c84c57dbae7d74a47640434f2678ae13a
Status: Downloaded newer image for mongo:4
4e2e51837df8ed226dd607f47f7923fc27800ea3b1095b8a4fd16f66fe8d8e97
init mongodb account admin and yapi
MongoDB shell version v4.0.9
connecting to: mongodb://127.0.0.1:27017/admin?gssapiServiceName=mongodb
2019-04-24T03:22:21.172+0000 E QUERY    [js] Error: couldn't connect to server 127.0.0.1:27017, connection attempt failed: SocketException: Error connecting to 127.0.0.1:27017 :: caused by :: Connection refused :
connect@src/mongo/shell/mongo.js:343:13
@(connect):2:6
exception: connect failed
inti mongodb done
init yapi db and start yapi server
5eaca2adb290fb948f7c2c1536cf1d209f92076376359221c555d39b5d6a7f6f
init yapi done
vagrant@ubuntu-xenial:~/yapi$ 
```

### init-mongodb 报错 ###

但是，这个时候，看到上面有错误提示了。


这时，`sudo docker logs --tail 10 yapi` 报错，说mongodb 认证错误。

这里 index.sh 中有 bash start.sh init-mongo 这一步，出错了。
也就是说，在 bash start.sh init-mongo 这一步的时候，出错了，没有成功。
决定手工执行一下吧。

打开 start.sh 看一下，init-mongo 函数的具体内容。执行一下。

```bash
cd docker-yapi
sudo docker ps
cat init-mongo.js
cat ../index.sh
sudo docker cp init-mongo.js mongod:/data
sudo docker exec -it mongod bash
# 在容器中执行 `mongo admin /data/init-mongo.js`
```

检查 mongodb 用户，确认 init-mongo 成功

```
$ mongo -u "admin" -p "admin123456"
> use admin
> show users
```

如果说有 yapi 用户，就表示，init-mongo 成功了。


### 启动 yapi 

```bash
vagrant@ubuntu-xenial:~/yapi/docker-yapi$ docker kill yapi && docker rm yapi
vagrant@ubuntu-xenial:~/yapi/docker-yapi$ sudo docker run -d -p 3001:3001 --name yapi --net tools-net --ip 172.18.0.3 yapi
b3611d11096e84324bdb31690878031607db1cfd52ab9758ce9bdd03df45c344
vagrant@ubuntu-xenial:~/yapi/docker-yapi$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
b3611d11096e        yapi                "/api/docker-entrypo…"   3 seconds ago       Up 3 seconds        0.0.0.0:3001->3001/tcp     yapi
4e2e51837df8        mongo:4             "docker-entrypoint.s…"   2 hours ago         Up 2 hours          0.0.0.0:27017->27017/tcp   mongod
vagrant@ubuntu-xenial:~/yapi/docker-yapi$ sudo docker logs --tail 10 yapi
log: 服务已启动，请打开下面链接访问:
http://127.0.0.1:3001/
log: mongodb load success...
vagrant@ubuntu-xenial:~/yapi/docker-yapi$
```

说明启动成功

```bash
vagrant@ubuntu-xenial:~/yapi/docker-yapi$ curl http://localhost:3001/
<!DOCTYPE html>
<html>
<head>
<meta  id="cross-request-sign"  charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
<meta name="keywords" content="yapi接口管理,api管理,接口管理,api,接口,接口文档,api文档,接口管理系统" />
<meta name="description" content="YApi 是高效、易用、功能强大的 api 管理平台，旨在为开发、产品、测试人员提供更优雅的接口管理服务。可以帮助开发者轻松创建、发布、维护 API，YApi 还为用户提供了优秀的交互体验，开发人员只需利用平台提供的接口数
据写入工具以及简单的点击操作就可以实现接口的管理。" />
<title>YApi-高效、易用、功能强大的可视化接口管理平台</title>
<link rel="icon" type="image/png" sizes="192x192" href="/image/favicon.png">
<script>
    document.write('<script src="/prd/assets.js?v=' + Math.random() + '"><\/script>');
</script>

<script>
    document.write('<link rel="stylesheet"  href="/prd/' + window.WEBPACK_ASSETS['index.js'].css + '" />');
</script>

</head>
<body>
<div id="yapi" style="height: 100%;"></div>


<script>
    document.write('<script src="/prd/' + window.WEBPACK_ASSETS['manifest'].js + '"><\/script>');
</script>
<script>
    document.write('<script src="/prd/' + window.WEBPACK_ASSETS['lib3'].js + '"><\/script>');
</script>
<script>
    document.write('<script src="/prd/' + window.WEBPACK_ASSETS['lib2'].js + '"><\/script>');
</script>
<script>
    document.write('<script src="/prd/' + window.WEBPACK_ASSETS['lib'].js + '"><\/script>');
</script>

<script>
    document.write('<script src="/prd/' + window.WEBPACK_ASSETS['index.js'].js + '"><\/script>');
</script>
</body>
</html>
vagrant@ubuntu-xenial:~/yapi/docker-yapi$
```

### 初始化 yapi接口管理

```bash
vagrant@ubuntu-xenial:~/yapi/docker-yapi$ sudo docker exec -it yapi sh
/ # which node
/usr/local/bin/node
/ # node /api/vendors/server/install.js
log: mongodb load success...
初始化管理员账号成功,账号名："ryan.miao@demo.com"，密码："ymfe.org"
/ # exit
```

chrome: http://192.168.168.162:3001
管理员登录,账号名："ryan.miao@demo.com"，密码："ymfe.org"
成功。

