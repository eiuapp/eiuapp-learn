+++
title = "Shell获取某目录下所有文件夹的名称"
date = 2018-11-23T00:00:00+08:00
lastmod = 2018-11-23T15:51:35+08:00
tags = ["shell", "file-name"]
categories = ["shell"]
draft = false
weight = 3001
+++

-   方法一

```
#!/bin/bash
dir=$(ls -l D:/temp/ |awk '/^d/ {print $NF}')
for i in $dir
do
    echo $i
done
```

-   方法二

```
#!/bin/bash
for dir in $(ls D:/tmep/)
do
    [ -d $dir ] && echo $dir
done
```

-   方法三

```
#!/bin/bash
ls -l D:/temp/ |awk '/^d/ {print $NF}'   ## 其实同方法一，直接就可以显示不用for循环
```

-   方法四

```
#!/bin/bash
ls -l  |awk '/^d/ {print $NF}'
## get file *.rst name
ls -l *.rst |awk '/^-/ {print $NF}'
```


## Ref {#ref}

-   <https://blog.csdn.net/sidely/article/details/40426725>
