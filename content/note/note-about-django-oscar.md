+++
title = "django-oscar(笔记)"
date = 2019-01-24T00:00:00-08:00
lastmod = 2019-01-25T18:29:17-08:00
tags = ["oscar", "django", "note"]
categories = ["oscar"]
draft = false
weight = 3001
+++

```shell
(oscar) ➜  django-oscar git:(master) make sandbox
pip install -r requirements.txt
Collecting Werkzeug==0.14.1 (from -r requirements.txt (line 4))
  Downloading https://files.pythonhosted.org/packages/20/c4/12e3e56473e52375aa29c4764e70d1b8f3efa6682bef8d0aae04fe335243/Werkzeug-0.14.1-py2.py3-none-any.whl (322kB)
    100% |████████████████████████████████| 327kB 8.8MB/s
Collecting django-debug-toolbar==1.11 (from -r requirements.txt (line 5))
  Downloading https://files.pythonhosted.org/packages/01/9a/3db232bd15882d90d3c53de1f34ce0a522327849593c9198899713267cfe/django_debug_toolbar-1.11-py2.py3-none-any.whl (201kB)
    100% |████████████████████████████████| 204kB 24.9MB/s
Collecting django-extensions==2.1.4 (from -r requirements.txt (line 6))
  Downloading https://files.pythonhosted.org/packages/e4/56/6a854a56732f7cb6a0393b8a32ae8a37b82b004e638b7b2f153b66733ce5/django_extensions-2.1.4-py2.py3-none-any.whl (217kB)
    100% |████████████████████████████████| 225kB 4.8MB/s
Collecting psycopg2<2.8,>=2.7 (from -r requirements.txt (line 7))
  Downloading https://files.pythonhosted.org/packages/63/54/c039eb0f46f9a9406b59a638415c2012ad7be9b4b97bfddb1f48c280df3a/psycopg2-2.7.7.tar.gz (427kB)
    100% |████████████████████████████████| 430kB 4.1MB/s
    Complete output from command python setup.py egg_info:
    running egg_info
    creating pip-egg-info/psycopg2.egg-info
    writing pip-egg-info/psycopg2.egg-info/PKG-INFO
    writing dependency_links to pip-egg-info/psycopg2.egg-info/dependency_links.txt
    writing top-level names to pip-egg-info/psycopg2.egg-info/top_level.txt
    writing manifest file 'pip-egg-info/psycopg2.egg-info/SOURCES.txt'

    Error: pg_config executable not found.

    pg_config is required to build psycopg2 from source.  Please add the directory
    containing pg_config to the $PATH or specify the full executable path with the
    option:

        python setup.py build_ext --pg-config /path/to/pg_config build ...

    or with the pg_config option in 'setup.cfg'.

    If you prefer to avoid building psycopg2 from source, please install the PyPI
    'psycopg2-binary' package instead.

    For further information please check the 'doc/src/install.rst' file (also at
    <http://initd.org/psycopg/docs/install.html>).


    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-install-e2o6_oa6/psycopg2/
Makefile:19: recipe for target 'install-python' failed
make: *** [install-python] Error 1
(oscar) ➜  django-oscar git:(master)

```
