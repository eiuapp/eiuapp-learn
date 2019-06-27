+++
title = "nodejs 一定要通过 nvm安装"
date = 2019-02-13T00:00:00+08:00
lastmod = 2019-02-15T23:00:03+08:00
tags = ["nodejs", "install", "nvm"]
categories = ["nodejs"]
draft = false
weight = 3001
+++

使用 nvm 管理不同版本的 node 与 npm


## step {#step}

补充说明：Mac 下通过 brew install nvm 所安装的 nvm ，由于安装路径不同，无法正确启用。建议使用 brew uninstall nvm 卸载掉之后，通过本文的方案重新安装一次。

nvm 是 Mac 下的 node 管理工具，有点类似管理 Ruby 的 rvm，如果是需要管理 Windows 下的 node，官方推荐是使用 nvmw 或 nvm-windows 。

以下具体说下 Mac 系统中的安装与使用细节（Windows 系统仅供类比参考）。


## 一、卸载已安装到全局的 node/npm {#一-卸载已安装到全局的-node-npm}

如果之前是在官网下载的 node 安装包，运行后会自动安装在全局目录，其中

-   node 命令在 \`/usr/local/bin/node\`
-   npm 命令在 \`/usr/local/bin/npm\` ，其实是 \`/usr/local/lib/node\_modules/npm/bin/npm-cli.js\` 的软链接
-   全局 node\_modules 目录在 \`/usr/local/lib/node\_modules/\`

安装 nvm 之后最好先删除下已安装的 node 和全局 node 模块：

```shell
npm ls -g --depth=0 #查看已经安装在全局的模块，以便删除这些全局模块后再按照不同的 node 版本重新进行全局安装

sudo rm -rf /usr/local/lib/node_modules #删除全局 node_modules 目录
sudo rm /usr/local/bin/node #删除 node
cd  /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm #删除全局 node 模块注册的软链
```


## 二、安装 nvm {#二-安装-nvm}

https://github.com/creationix/nvm

```shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
```

安装完成后请重新打开终端环境，Mac 下推荐使用 oh-my-zsh 代替默认的 bash shell。

### 报错 The program 'rbenv' is currently not installed


如果 nvm 安装过程中, 报出如下错误

```
The program 'rbenv' is currently not installed. You can install it by typing:
sudo apt install rbenv
```
那么说明没有安装成功的。要依据提示做一下。

```
u@DESKTOP-APB1HCJ:~$ sudo apt install rbenv 
```

### 检查

```
u@DESKTOP-APB1HCJ:~$ which nvm
DESKTOP-APB1HCJ% nvm --version
0.34.0
DESKTOP-APB1HCJ% nvm version
v11.14.0
DESKTOP-APB1HCJ%
```


## 三、安装切换各版本 node/npm {#三-安装切换各版本-node-npm}

```shell
nvm install stable #安装最新稳定版 node，现在是 5.0.0
nvm install 4.2.2 #安装 4.2.2 版本
nvm install 0.12.7 #安装 0.12.7 版本

# 特别说明：以下模块安装仅供演示说明，并非必须安装模块
nvm use 0 #切换至 0.12.7 版本
npm install -g mz-fis #安装 mz-fis 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v0.12.7/lib/mz-fis
nvm use 4 #切换至 4.2.2 版本
npm install -g react-native-cli #安装 react-native-cli 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v4.2.2/lib/react-native-cli

nvm alias default 0.12.7 #设置默认 node 版本为 0.12.7
```

查看已经安装版本

```
nvm ls
```

帮助

```
nvm --help
```

## 四、使用 .nvmrc 文件配置项目所使用的 node 版本 {#四-使用-dot-nvmrc-文件配置项目所使用的-node-版本}

如果你的默认 node 版本（通过 nvm alias 命令设置的）与项目所需的版本不同，则可在项目根目录或其任意父级目录中创建 .nvmrc 文件，在文件中指定使用的 node 版本号，例如：

```shell
cd <项目根目录>  #进入项目根目录
echo 4 > .nvmrc #添加 .nvmrc 文件
nvm use #无需指定版本号，会自动使用 .nvmrc 文件中配置的版本
node -v #查看 node 是否切换为对应版本
```


## 五、nvm 与 n 的区别 {#五-nvm-与-n-的区别}

node 版本管理工具还有一个是 TJ 大神的 n 命令，n 命令是作为一个 node 的模块而存在，而 nvm 是一个独立于 node/npm 的外部 shell 脚本，因此 n 命令相比 nvm 更加局限。

由于 npm 安装的模块路径均为 /usr/local/lib/node\_modules ，当使用 n 切换不同的 node 版本时，实际上会共用全局的 node/npm 目录。 因此不能很好的满足『按不同 node 版本使用不同全局 node 模块』的需求。

因此建议各位尽早开始使用 nvm ，以免出现全局模块无法更新的问题。

当然，如果你用的是 windows 操作系统，我只能说 …… 朋友，能不能早点换个 Mac


## Ref {#ref}

-   <http://www.cnblogs.com/kaiye/p/4937191.html>
