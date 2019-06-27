+++
title = "Ubuntu Desktop 环境安装"
date = 2018-11-29T17:50:00+08:00
lastmod = 2018-11-29T23:47:50+08:00
tags = ["desktop", "ubuntu", "setup"]
categories = ["ubuntu"]
draft = false
weight = 3002
+++

## 安装Ubuntu {#安装ubuntu}

**全部安装流程：**

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/21flowinstall.png)

推荐的Ubuntu环境：

-   版本：Ubuntu 16.04 LTS
-   语言：简体中文
-   时区：Shanghai
-   硬件：64位，4G内存以上

> 引用官方安装教程：[点击查看](https://www.ubuntu.com/download/desktop/install-ubuntu-desktop)


## 更新软件源 {#更新软件源}

**apt源**

操作步骤如下：

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/21211flowaptupdatesource.png)

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/21212setting.png)

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/21213softandupdate.png)

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/21214othersites.png)

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/21215bestserver.png)

进行上述操作之后，系统会花几分钟时间进行服务器速度的测试。测试完毕之后，点击『选择服务器』，后面根据提示输入密码和重新载入软件信息即可。

**pip源**

创建pip的配置文件，在终端中执行如下命令创建.pip文件夹

```sh
mkdir ~/.pip/
```

再执行如下命令创建pip.conf文件并编辑

```sh
vim ~/.pip/pip.conf
```

这时候会出现pip.conf的编辑窗口，按字母键a进入到编辑模式，这时候内容为空，并把如下内容输入到编辑框里面

```sh
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/21221vimpipconfig.png)

编辑完后保存并退出vim： **ESC** -> **shift+;** -> **x** -> **Enter**


## 安装Python {#安装python}

下载Anaconda2并安装，在终端中顺序执行下面3行命令

```sh
wget https://repo.continuum.io/archive/Anaconda2-4.0.0-Linux-x86_64.sh
```

```sh
chmod +x Anaconda2-4.0.0-Linux-x86_64.sh
```

```sh
./Anaconda2-4.0.0-Linux-x86_64.sh
```

然后开始进行安装：

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/2131anacondainstall.png)

后面根据提示按回车或者输入yes即可，要注意一下在提示是否要在.bashrc文件中更新PATH变量时，一定要输入Yes：

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/2132recommandyes.png)

安装完毕之后执行如下命令让bash的配置文件即时生效

```sh
source ~/.bashrc
```

执行完之后执行python命令进行验证，如果安装成功会出现 Anaconda 4.0.0
的字样：

![figure](http://cdn.sinacloud.net/vnpydoc/ubuntuinstall/2133verifyifsuccess.png)

由于vnpy最近对python3的兼容，需要安装future模块，[ImportError:
No module named 'Queue'](https://github.com/vnpy/vnpy/issues/639)

```sh
pip install future
```


## 安装依赖项 {#安装依赖项}

执行如下两行命令分别安装应用软件和pip的库

```sh
sudo apt-get install mongodb
sudo apt-get install libboost-all-dev
sudo apt-get install cmake
sudo apt-get install git
sudo apt-get install libsnappy-dev
sudo apt-get install python-snappy
```

最后两个包的原因请看 <https://github.com/vnpy/vnpy/issues/639>
如果安装的时候报找不到mongodb，就更新一下apt的源文件信息 sudo apt-get
update


## 安装和配置MongoDB {#安装和配置mongodb}

安装MongoDB

```sh
sudo apt-get install mongodb
```

经过测试发现在ubuntu16.04
LTS下安装MongoDB后默认就安装为了自启动的系统服务。

如果大家想查看MongoDB的数据文件和日志文件的具体位置，可以用如下命令进行查看：

```sh
head /etc/mongodb.conf
```

会显示如下内容：

```
# mongodb.conf

# Where to store the data.
dbpath=/var/lib/mongodb

#where to log
logpath=/var/log/mongodb/mongodb.log
```

dbpath 是数据文件存储路径，logpath 是日志文件存储路径。

MongoDB客户端方面推荐使用Robomongo，下载[官方压缩包](https://robomongo.org/)，解压后进入到bin目录，双击robomongo即可。


## 准备编译工具 {#准备编译工具}

```sh
apt-get install build-essential
apt-get install python-dev
```


## 中文字体安装（如果使用的是英文版ubuntu Linux） {#中文字体安装如果使用的是英文版ubuntu-linux}

如果系统中没有安装中文字体，图形界面中的中文就会显示为方块。解决方案：

```sh
sudo apt install fonts-wqy-zenhei
```


## Ref {#ref}

-   <https://github.com/vnpy/vnpy/wiki/Ubuntu%E7%8E%AF%E5%A2%83%E5%AE%89%E8%A3%85>
