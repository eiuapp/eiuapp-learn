windows wsl 子系统 
https://www.jianshu.com/p/bc38ed12da1d 此文中的环境与我的大体一致。

## env

win10 版本： 1703 (查看版本可 cmd 下输入`winver`)

## 找到 子系统 目录

在 文件夹下 输入 `%localappdata%`, 然后搜索框输入： `rootfs` , 则会出现结果。

我的是在  `%localappdata%\lxss\rootfs` 。那么说明 wsl 子系统 目录 是 `%localappdata%\lxss` 


## 子系统环境(lxss目录) 备份


那最终powershell 中 运行命令如下（运行过程中视情况：1.D选择复制目录;2.选择A全选择）：

```
xcopy .\lxss .\lxss.20190306.bak /E/C
```


毕竟爱折腾，难免会把子系统环境(lxss目录)玩坏掉，因此干正事前最好先备份下以便快速还原。注意，不要直接右键复制或者打包，可能会导致文件权限丢失的。

powershell 中 运行：

```
> xcopy .\lxss  .\lxss.bak /E
```

但是，因为某些文件，会导致`文件创建错误 - 系统找不到指定的文件。` 这样的错误：

```
Local master + = $ xcopy .\lxss\root\.nvm\test .\lxsstest /E
目标 .\lxsstest 是文件名
还是目录名
(F = 文件，D = 目录)? d
.\lxss\root\.nvm\test\common.sh
.\lxss\root\.nvm\test\fast\nvm should remove the last trailing slash in $NVM_DIR
.\lxss\root\.nvm\test\fast\Running #0022nvm alias#0022 should create a file in the alias directory.
文件创建错误 - 系统找不到指定的文件。

Local master + = $ ls /E
```

查看 xcopy 参数，https://blog.csdn.net/zhihui1017/article/details/50621747 ，可能 `/C 即使有错误，也继续复制。` 能帮助我们


```
Local master + = $ xcopy .\lxss\root\.nvm\test .\lxsstest3 /E/C
目标 .\lxsstest3 是文件名
还是目录名
(F = 文件，D = 目录)? d
.\lxss\root\.nvm\test\common.sh
.\lxss\root\.nvm\test\fast\nvm should remove the last trailing slash in $NVM_DIR
.\lxss\root\.nvm\test\fast\Running #0022nvm alias#0022 should create a file in the alias directory.
文件创建错误 - 系统找不到指定的文件。

.\lxss\root\.nvm\test\fast\Running #0022nvm current#0022 should display current nvm environment.
文件创建错误 - 系统找不到指定的文件。

.\lxss\root\.nvm\test\fast\Running #0022nvm deactivate#0022 should unset the nvm environment variables.
文件创建错误 - 系统找不到指定的文件。
...
...
...
```

那最终命令如下：

```
xcopy .\lxss .\lxss.20190306.bak /E/C
```

## Error 0x80070005 on startup 

https://github.com/Microsoft/WSL/issues/651

Also had this issue after running Oracle instant client. Just like others above, restarting into safe mode (hold shift and choose restart on the login screen or start menu) and deleting %LOCALAPPDATA%\lxss\temp fixed the problem.

也就是删除 `%LOCALAPPDATA%\lxss\temp` 就可以了
## ref
- https://www.jianshu.com/p/bc38ed12da1d
