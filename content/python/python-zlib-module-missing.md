---
title: "Python Zlib Module Missing"
date: 2018-06-04T14:22:38+08:00
draft: false
tags: ["python"]
categories: ["python"]
subtitle: "python安装包的过程中提示`requires the (missing) zlib module`"
descriptions: ""
bigimg:
---


```
 Traceback (most recent call last):
  File "setup.py", line 94, in <module>
    scripts = scripts,
  File "/usr/local/lib/python2.7/distutils/core.py", line 152, in setup
    dist.run_commands()
  File "/usr/local/lib/python2.7/distutils/dist.py", line 953, in run_commands
    self.run_command(cmd)
  File "/usr/local/lib/python2.7/distutils/dist.py", line 972, in run_command
    cmd_obj.run()
  File "/home/rohan/setuptools-0.6c11/setuptools/command/install.py", line 76, in run
    self.do_egg_install()
  File "/home/rohan/setuptools-0.6c11/setuptools/command/install.py", line 96, in do_egg_install
    self.run_command('bdist_egg')
  File "/usr/local/lib/python2.7/distutils/cmd.py", line 326, in run_command
    self.distribution.run_command(command)
  File "/usr/local/lib/python2.7/distutils/dist.py", line 972, in run_command
    cmd_obj.run()
  File "/home/rohan/setuptools-0.6c11/setuptools/command/bdist_egg.py", line 236, in run
    dry_run=self.dry_run, mode=self.gen_header())
  File "/home/rohan/setuptools-0.6c11/setuptools/command/bdist_egg.py", line 527, in make_zipfile
    z = zipfile.ZipFile(zip_filename, mode, compression=compression)
  File "/usr/local/lib/python2.7/zipfile.py", line 651, in __init__
    "Compression requires the (missing) zlib module"
RuntimeError: Compression requires the (missing) zlib module
```



## Env

- os: centos6.9
- python: 2.7.9

## Step

first install the companents with the following command
```
yum install -y zlib zlib-devel
```
then remake python
```
make && make install
```

## Ref

- https://stackoverflow.com/questions/3905615/zlib-module-missing