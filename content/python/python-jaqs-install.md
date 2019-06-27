---
title: "Jaqs安装笔记"
date: 2018-10-16T11:38:26+08:00
draft: false
tags: ["jaqs", "python"]
categories: ["python"]
subtitle: "用 python3 安装 jaqs"
descriptions: ""
bigimg:
---

## Env

- 


## Step

```
## 下载并安装 Anaconda
chmod +x Anaconda3-5.3.0-MacOSX-x86_64.sh
which sh
sh ./Anaconda3-5.3.0-MacOSX-x86_64.sh
## 配置环境
cat /Users/tomtsang/.bash_profile
source /Users/tomtsang/.bash_profile
which python
which python3
which python2  ## 这个还是原来的路径
python3
python
which ipython
ipython  
workon quant-py3  ## 验证，不影响其它环境
which python
which python3
which python2
which deactivate  
deactivate  ## 退出 virtualenv
## 安装依赖 snappy
brew install snappy
CPPFLAGS="-I/usr/local/include -L/usr/local/lib" pip install python-snappy
pip install PyHamcrest
## 安装 jaqs
pip install jaqs
## 验证
ipython ## 输入 import jaqs
## 
```

## Ref

- https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/
- https://jaqs.readthedocs.io/zh_CN/latest/install.html
- https://github.com/quantOS-org/JAQS
- https://github.com/andrix/python-snappy#frequently-asked-questions