+++
title = "docker login failure,  unauthorized: incorrect username or password"
date = 2018-12-15T00:00:00+08:00
lastmod = 2018-12-15T17:31:52+08:00
tags = ["docker", "login"]
categories = ["docker"]
draft = false
weight = 3001
+++

报错中出现类似“unauthorized: incorrect username or password”的话。

```shell
➜  saleor git:(master) docker-compose run web python3 manage.py migrate
Pulling db (library/postgres:latest)...
latest: Pulling from library/postgres
a5a6f2f73cd8: Already exists
e50fbea8af5a: Pull complete
73b4855ad326: Pull complete
ERROR: Get https://registry-1.docker.io/v2/library/postgres/manifests/latest: unauthorized: incorrect username or password
➜  saleor git:(master)
```

重新使用 docker.com 中的用户名登陆，而不是邮箱登陆。

```shell
➜  saleor git:(master) docker logout
Removing login credentials for https://index.docker.io/v1/
➜  saleor git:(master) docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: tomtsang
Password:
Login Succeeded
➜  saleor git:(master)
```

这样后续就正常了。


## Ref {#ref}

-   <https://forums.docker.com/t/unauthorized-incorrect-username-or-password/35677/5>
