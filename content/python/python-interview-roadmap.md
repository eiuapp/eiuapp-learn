+++
title = "python面试准备roadmap"
date = 2018-12-16T00:00:00+08:00
lastmod = 2018-12-16T16:43:45+08:00
tags = ["python", "interview"]
categories = ["python"]
draft = false
weight = 3001
+++

## step1 {#step1}

<https://www.cnblogs.com/Vito2008/p/5044251.html>

```python
#!/Users/tomtsang/.virtualenvs/quant-py3/bin/python
# -*- coding:utf-8 -*-
# Author: zengyunlong

# 补充缺失的代码

def print_directory_contents(sPath):
    """
    这个函数接受文件夹的名称作为输入参数，
    返回该文件夹中文件的路径，
    以及其包含文件夹中文件的路径。

    """
    import os
    for sChild in os.listdir(sPath):
        sChildPath = os.path.join(sPath , sChild)
        if os.path.isdir(sChildPath):
            print_directory_contents(sChildPath)
        else:
            print(sChildPath)
print("this is before if __name__:%s"%__name__)
if __name__=='__main__':
    print("this is after if __name__:%s"%__name__)
    print(print_directory_contents("."))

```


## <span class="org-todo todo TODO">TODO</span> step2 {#step2}

<https://www.cnblogs.com/goodhacker/p/3366618.html>


## <span class="org-todo todo TODO">TODO</span> step3 {#step3}

<https://blog.csdn.net/weixin%5F41666747/article/details/79942847>
