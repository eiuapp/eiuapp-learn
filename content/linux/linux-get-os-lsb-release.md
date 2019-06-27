+++
title = "ubuntu 获取 系统版本信息"
date = 2019-01-25T00:00:00-08:00
lastmod = 2019-01-25T18:28:36-08:00
tags = ["linux"]
categories = ["linux"]
draft = false
weight = 3003
+++

```shell
(oscar) ➜  proxy-os git:(master) ✗ lsb_release --help
Usage: lsb_release [options]

Options:
  -h, --help         show this help message and exit
  -v, --version      show LSB modules this system supports
  -i, --id           show distributor ID
  -d, --description  show description of this distribution
  -r, --release      show release number of this distribution
  -c, --codename     show code name of this distribution
  -a, --all          show all of the above information
  -s, --short        show requested information in short format
(oscar) ➜  proxy-os git:(master) ✗ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 16.04.5 LTS
Release:        16.04
Codename:       xenial
(oscar) ➜  proxy-os git:(master) ✗ lsb_release -s -c
xenial
(oscar) ➜  proxy-os git:(master) ✗
```
