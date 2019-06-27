+++
title = "shell批量替换文件扩展名"
date = 2018-11-23T00:00:00+08:00
lastmod = 2018-11-23T16:23:38+08:00
tags = ["shell", "file-name"]
categories = ["shell"]
draft = false
weight = 3002
+++

早上本想将一些照片上传到相册中，但是由于所有照片的扩展名都是JPG而不是小写的jpg，因此造成了“格式不正确”而不能上传照片。此刻就产生了这样一个问题：使用shell脚本如何批量将所有文件的扩展名JPG都改成小写的jpg？

既然要批量替换文件名，那么肯定得用一个for循环依次遍历指定目录下的每个文件。对于每个文件，假如该文件的名称为name.oldext，那么我们必须原始文件名中挖出name，再将它与新的文件扩展名newext拼接形成新的文件名name.newext。依照这样的思路，就诞生了下面的脚本：

```
#!/bin/bash
oldext="JPG"
newext="jpg"

dir="/home/edsionte/mypic"
cd $dir

for file in $(ls $dir | grep .$oldext)

do
    name=$(ls $file | cut -d. -f1)
    mv $file ${name}.$newext
done
```

下面对针对这个程序作简单说明：

1.变量oldext和newext分别指定旧的扩展名和新的扩展名。dir指定文件所在目录；

2.“ls $dir | grep .$oldext”用来在指定目录dir中获取扩展名为旧扩展名的所有文件；

3.在循环体内先利用cut命令将文件名中“.”之前的字符串剪切出来，并赋值给name变量；接着将当前的文件名重命名为新的文件名。

通过这个脚本，所有照片的扩展名都成功修改。为了使这个脚本更具有通用型，我们可以增加几条read命令实现脚本和用户之间的交互。改进版的脚本如下：

```
#!/bin/bash

read -p "old extension:" oldext
read -p "new extension:" newext
read -p "The directory:" dir
cd $dir

for file in $(ls $dir | grep .$oldext)

do
    name=$(ls $file | cut -d. -f1)
    mv $file ${name}.$newext
    echo "$name.$oldext ====> $name.$newext"
done
echo "all files has been modified."
```

修改后的脚本可以批量修改任意扩展名。done。


## Ref {#ref}

-   <https://note.youdao.com/web/#/file/recent/note/wcp1542961015800180/>
