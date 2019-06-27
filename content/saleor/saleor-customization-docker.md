+++
title = "docker 安装 saleor"
date = 2019-01-31T00:00:00+08:00
lastmod = 2019-01-31T22:54:59+08:00
tags = ["saleor", "install"]
categories = ["saleor"]
draft = false
weight = 3001
+++

<https://docs.getsaleor.com/en/latest/customization/docker.html>


## 安装后 {#安装后}

完成步骤后的效果如下：

```shell
(saleor) ➜  saleor git:(master) ✗ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                    NAMES
202bd2594ad9        saleor_web          "python manage.py ru…"   About a minute ago   Up 33 seconds       0.0.0.0:8000->8000/tcp   saleor_web_1
af1d1c556b35        saleor_celery       "celery -A saleor wo…"   About a minute ago   Up 33 seconds       8000/tcp                 saleor_celery_1
aa067abbd302        postgres:latest     "docker-entrypoint.s…"   About a minute ago   Up 34 seconds       0.0.0.0:5432->5432/tcp   saleor_db_1
dfb447947378        redis:latest        "docker-entrypoint.s…"   About a minute ago   Up 33 seconds       0.0.0.0:6379->6379/tcp   saleor_redis_1
(saleor) ➜  saleor git:(master) ✗ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
saleor_celery       latest              ed5d34462667        3 minutes ago       619MB
saleor_web          latest              ed5d34462667        3 minutes ago       619MB
<none>              <none>              df1c689ebe30        About an hour ago   619MB
<none>              <none>              0bd648a8810c        About an hour ago   1.42GB
<none>              <none>              f70f27cf8740        About an hour ago   1.39GB
<none>              <none>              c0e63021a2e5        2 hours ago         965MB
node                10                  c481a0ff9f4f        14 hours ago        897MB
postgres            latest              d03e2a8f3ed4        17 hours ago        312MB
python              3.6-slim            e9b00f8a90d8        5 days ago          138MB
python              3.6                 5b0503f864f4        5 days ago          922MB
redis               latest              82629e941a38        8 days ago          95MB
hello-world         latest              fce289e99eb9        4 weeks ago         1.84kB
(saleor) ➜  saleor git:(master) ✗
```


## error1 {#error1}

如果打开 <https://127.0.0.1:8000> 出现了如下错误

```
relation "django_site" does not exist LINE 1: ..."django_site"."domain", "django_site"."name" FROM "django_si...
```

则说明是没有进行 \`./manage.py migrate\` 这一命令。也就是说，很可能，你忘记进行本
小节的第2步骤。

```shell
docker-compose run web python3 manage.py migrate
docker-compose run web python3 manage.py collectstatic
docker-compose run web python3 manage.py populatedb --createsuperuser
```

当然，如果你确定，你是进行了上面的3个命令，还是有之前的那个报错，那么请进入
container，然后执行命令。

```shell
(saleor) ➜  saleor git:(master) ✗ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
202bd2594ad9        saleor_web          "python manage.py ru…"   30 minutes ago      Up 29 minutes       0.0.0.0:8000->8000/tcp   saleor_web_1
af1d1c556b35        saleor_celery       "celery -A saleor wo…"   30 minutes ago      Up 29 minutes       8000/tcp                 saleor_celery_1
aa067abbd302        postgres:latest     "docker-entrypoint.s…"   30 minutes ago      Up 29 minutes       0.0.0.0:5432->5432/tcp   saleor_db_1
dfb447947378        redis:latest        "docker-entrypoint.s…"   30 minutes ago      Up 29 minutes       0.0.0.0:6379->6379/tcp   saleor_redis_1
(saleor) ➜  saleor git:(master) ✗ docker exec -it 202bd2594ad9 bash
root@202bd2594ad9:/app# ./manage.py migrate
root@202bd2594ad9:/app# ./manage.py collectstatic
root@202bd2594ad9:/app# ./manage.py populatedb --createsuperuser
```

执行完成后，回到浏览器中去登陆 <http://192.168.1.103:8000/zh-hans/account/login/> ，应该是没有问题的。

admin account : admin@example.com
password : admin


## Ref {#ref}

-   <https://stackoverflow.com/questions/23925726/django-relation-django-site-does-not-exist>
