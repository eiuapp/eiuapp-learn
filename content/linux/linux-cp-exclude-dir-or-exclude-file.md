
在linux中，如果你想cp一个文件夹，但是，又不想cp此文件夹下的某些文件或某些文件夹，则请往下看。


## step

### 方法一：cp & ls & grep

此方法，最好，简单直接。主要就是依赖grep去除

```shell
cd spacemacs-private/ && cp -rf `ls -a . | grep -E -v "^(.|..|.git|.gitignore)$"` ../../../submodule/test/
```

### 方法二：cp & find

说明：/home目录里面有data目录，data目录里面有a、b、c、d、e五个目录，现在要把data目录里面除过e目录之外的所有目录拷贝到/bak目录中 

终端命令行下执行以下命令

```shell
cp -R `find /home/data -type d  -path /home/data/e  -prune -o -print | sed 1d ` /bak
```

脚本实现
脚本存放路径/home/osyunwei.sh

```shell
$ cat /home/osyunwei.sh   #编辑脚本，添加下面的代码
#!/bin/sh
cp -R `find /home/data -type d  -path /home/data/e  -prune -o -print | sed 1d ` /bak
$ chmod +x /home/osyunwei.sh   #添加脚本执行权限
$ cd  /home  #进入脚本存放目录
$ ./osyunwei.sh   #执行脚本
``` 

### 方法三：rsync

使用cp命令复制的时候，只能排除一个目录不被复制，如果想排除两个或者多个目录的话，就需要使用rsync命令来实现了，看下面的例子
如果要排除/home/data目录下面的a、b、c、三个目录，同时拷贝其它所有目录，执行以下命令。

```shell
yum install rsync   #安装rsync
rsync -av --exclude data/a  --exclude data/b  --exclude data/c  data   /bak
```

注意：--exclude后面的路径不能为绝对路径，必须为相对路径才可以，否则出错。

实际操作中，很容易发现问题，此方法，有副作用，不是函数式的（多次执行，无效果）

```shell
➜  latest git:(master) ✗ rsync -av --exclude spacemacs-private/.git  --exclude spacemacs-private/.gitignore  ../../submodule/test/
sending incremental file list
drwxrwxr-x          4,096 2019/01/23 20:23:34 .
-rw-rw-r--          1,430 2019/01/15 22:22:59 README.org

sent 85 bytes  received 125 bytes  420.00 bytes/sec
total size is 1,430  speedup is 6.81
➜  latest git:(master) ✗ ls
spacemacs-private
➜  latest git:(master) ✗ rsync -av --exclude spacemacs-private/.git  --exclude spacemacs-private/.gitignore  ../../submodule/test/
sending incremental file list
drwxrwxr-x          4,096 2019/01/23 20:24:15 .

sent 56 bytes  received 64 bytes  240.00 bytes/sec
total size is 0  speedup is 0.00
➜  latest git:(master) ✗ rsync -av --exclude spacemacs-private/.git  --exclude spacemacs-private/.gitignore  ../../submodule/test/
sending incremental file list
drwxrwxr-x          4,096 2019/01/23 20:24:15 .

sent 56 bytes  received 64 bytes  240.00 bytes/sec
total size is 0  speedup is 0.00
➜  latest git:(master) ✗ 
```

## ref

- https://yq.aliyun.com/ziliao/54339
- https://www.osyunwei.com/archives/2626.html
