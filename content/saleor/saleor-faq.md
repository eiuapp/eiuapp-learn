---
title: "Saleor"
date: 2018-04-15T23:58:27+08:00
draft: false 
tags: ["shop"]
categories: ["shop"]
subtitle: ""
descriptions: ""
bigimg:
gitment: true
---

关于 saleor 项目

## env


- IP: home, 192.168.31.109
- OS: ubuntu, 16.04.03


## step

### FAQ-1

#### `pip install uwsgi==2.0.17`时出错: `lto1: fatal error: bytecode stream generated with LTO version 6.0 instead of the expected 4.1`

见 `python-install-uwsgi` 一文.

### FAQ-2 

这一小节是一个大大的错误, 可直接看下一小节

```
export SECRET_KEY='**********'
export DATABASE_URL='*********'
sudo vi /etc/postgresql/9.6/main/pg_hba.conf 
ls /etc/init.d/postgresql
sudo /etc/init.d/postgresql reload
createuser -P -s -e saleor # 报错
createuser -P -s -e saleor -U postgres
python manage.py migrate # 报错
which createdb
createdb saleorBillions  # 报错
createdb saleorBillions -U postgres
python manage.py migrate # 报错
```
还是报错,如下
```
(saleor-a-1) tom@saleor-a:~/saleor$ python manage.py migrate
Traceback (most recent call last):
  File "manage.py", line 10, in <module>
    execute_from_command_line(sys.argv)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/core/management/__init__.py", line 371, in execute_from_command_line
    utility.execute()
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/core/management/__init__.py", line 317, in execute
    settings.INSTALLED_APPS
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/conf/__init__.py", line 56, in __getattr__
    self._setup(name)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/conf/__init__.py", line 43, in _setup
    self._wrapped = Settings(settings_module)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/conf/__init__.py", line 106, in __init__
    mod = importlib.import_module(self.SETTINGS_MODULE)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/importlib/__init__.py", line 126, in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
  File "<frozen importlib._bootstrap>", line 994, in _gcd_import
  File "<frozen importlib._bootstrap>", line 971, in _find_and_load
  File "<frozen importlib._bootstrap>", line 955, in _find_and_load_unlocked
  File "<frozen importlib._bootstrap>", line 665, in _load_unlocked
  File "<frozen importlib._bootstrap_external>", line 678, in exec_module
  File "<frozen importlib._bootstrap>", line 219, in _call_with_frames_removed
  File "/home/tom/saleor/saleor/settings.py", line 42, in <module>
    conn_max_age=600)}
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/dj_database_url.py", line 53, in config
    config = parse(s, engine, conn_max_age)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/dj_database_url.py", line 110, in parse
    engine = SCHEMES[url.scheme] if engine is None else engine
KeyError: ''
(saleor-a-1) tom@saleor-a:~/saleor$
```

走到这里, 说明已经要深入到代码内部了,我感到可怕...

重新梳理一下..

发现, 原来在项目中, 有一个 `common.env` 文件, 我去, 原来, 配置文件在这里

#### 去除原来的配置

去除 DATABASE_URL

```
(saleor-a-1) tom@saleor-a:~/saleor$ unset DATABASE_URL
(saleor-a-1) tom@saleor-a:~/saleor$ echo $DATABASE_URL

(saleor-a-1) tom@saleor-a:~/saleor$
```

### 开始吧

#### 创建database: `saleor`
```
(saleor-a-1) tom@saleor-a:~/saleor$ createdb saleor -U postgres
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = "en_US:en",
	LC_ALL = (unset),
	LC_PAPER = "zh_CN.UTF-8",
	LC_ADDRESS = "zh_CN.UTF-8",
	LC_MONETARY = "zh_CN.UTF-8",
	LC_NUMERIC = "zh_CN.UTF-8",
	LC_TELEPHONE = "zh_CN.UTF-8",
	LC_IDENTIFICATION = "zh_CN.UTF-8",
	LC_MEASUREMENT = "zh_CN.UTF-8",
	LC_TIME = "zh_CN.UTF-8",
	LC_NAME = "zh_CN.UTF-8",
	LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("en_US.UTF-8").
(saleor-a-1) tom@saleor-a:~/saleor$
```

#### 修改 `common.env` 文件

```
tom@saleor-a:~/saleor$ cat common.env
DATABASE_URL=postgres://saleor:saleor@db/saleor
DEFAULT_FROM_EMAIL=noreply@example.com
ELASTICSEARCH_URL=http://search:9200
OPENEXCHANGERATES_API_KEY
CACHE_URL=redis://redis:6379/0
CELERY_BROKER_URL=redis://redis:6379/1
SECRET_KEY=i2aru5o8MJrN4yT7
JWT_VERIFY_EXPIRATION=True
tom@saleor-a:~/saleor$
```

#### 再次 `python manage.py migrate` 

```
(saleor-a-1) tom@saleor-a:~/saleor$ python manage.py migrate
System check identified some issues:

WARNINGS:
saleor.W001: Session caching cannot work with locmem backend
	HINT: User sessions need to be globally shared, use a cache server like Redis.
Operations to perform:
  Apply all migrations: account, auth, cart, contenttypes, discount, django_celery_results, django_prices_openexchangerates, impersonate, menu, order, page, product, sessions, shipping, site, sites, social_django
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0001_initial... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying account.0001_initial... OK
  Applying account.0002_auto_20150907_0602... OK
  Applying account.0003_auto_20151104_1102... OK
  Applying account.0004_auto_20160114_0419... OK
  Applying account.0005_auto_20160205_0651... OK
  Applying account.0006_auto_20160829_0819... OK
  Applying account.0007_auto_20161115_0940... OK
  Applying account.0008_auto_20161115_1011... OK
  Applying account.0009_auto_20170206_0407... OK
  Applying account.0010_auto_20170919_0839... OK
  Applying account.0011_auto_20171110_0552... OK
  Applying account.0012_auto_20171117_0846... OK
  ...
  ...
  ...
  Applying social_django.0003_alter_email_max_length... OK
  Applying social_django.0004_auto_20160423_0400... OK
  Applying social_django.0005_auto_20160727_2333... OK
  Applying social_django.0006_partial... OK
  Applying social_django.0007_code_timestamp... OK
  Applying social_django.0008_partial_timestamp... OK
(saleor-a-1) tom@saleor-a:~/saleor$
```
到这里, 说明我们是真的安装完成了.

### FAQ-3, 再次启动

再次启动时, 报`The SECRET_KEY setting must not be empty.`
如下. 
```
(saleor-a-1) tom@saleor-a:~/saleor$ python manage.py runserver
Traceback (most recent call last):
  File "manage.py", line 10, in <module>
    execute_from_command_line(sys.argv)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/core/management/__init__.py", line 371, in execute_from_command_line
    utility.execute()
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/core/management/__init__.py", line 365, in execute
    self.fetch_command(subcommand).run_from_argv(self.argv)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/core/management/base.py", line 288, in run_from_argv
    self.execute(*args, **cmd_options)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/core/management/commands/runserver.py", line 61, in execute
    super().execute(*args, **options)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/core/management/base.py", line 335, in execute
    output = self.handle(*args, **options)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/core/management/commands/runserver.py", line 70, in handle
    if not settings.DEBUG and not settings.ALLOWED_HOSTS:
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/conf/__init__.py", line 56, in __getattr__
    self._setup(name)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/conf/__init__.py", line 43, in _setup
    self._wrapped = Settings(settings_module)
  File "/home/tom/.virtualenvs/saleor-a-1/lib/python3.6/site-packages/django/conf/__init__.py", line 125, in __init__
    raise ImproperlyConfigured("The SECRET_KEY setting must not be empty.")
django.core.exceptions.ImproperlyConfigured: The SECRET_KEY setting must not be empty.
(saleor-a-1) tom@saleor-a:~/saleor$
```

再次启动时, 要先运行, `export SECRET_KEY='i2aru5o8MJrN4yT7'`

```
(saleor-a-1) tom@saleor-a:~/saleor$ export SECRET_KEY='i2aru5o8MJrN4yT7'
(saleor-a-1) tom@saleor-a:~/saleor$ python manage.py runserver
Performing system checks...

System check identified some issues:

WARNINGS:
saleor.W001: Session caching cannot work with locmem backend
	HINT: User sessions need to be globally shared, use a cache server like Redis.

System check identified 1 issue (0 silenced).
April 23, 2018 - 23:18:58
Django version 2.0.3, using settings 'saleor.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
(saleor-a-1) tom@saleor-a:~/saleor$
```
## ref

- [saleor-installation-linux](https://saleor.readthedocs.io/en/latest/gettingstarted/installation-linux.html)
- [解决createdb: could not connect to database postgres: FATAL: Peer authentication failed for user "postgres"](https://www.cnblogs.com/terrysun/archive/2012/11/30/2796479.html) 
