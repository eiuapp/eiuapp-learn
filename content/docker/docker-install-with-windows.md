

win10家庭版安装Docker

https://blog.csdn.net/tidu2chengfo/article/details/84892915

## env

- os: windows 10 HOME

## step

### 安装前准备

#### 进入任务管理器看虚拟化是否已启用。 

如果你满足Docker for Windows的环境条件了，那么首先检查电脑的虚拟化开启了没有：进入任务管理器（ctrl+alt+delete），点击性能->cpu ,查看虚拟化是否已启用，如果虚拟化是已禁用，那么你需要重启电脑进入bios开启虚拟化（我们的发的笔记本cpu都是支持虚拟化的，重启时进入bios按esc -> 再按f12 -> 去开启虚拟化）

开启虚拟化重启后，进入任务管理器看虚拟化是否已启用。 

![](https://img-blog.csdn.net/20180308112539527?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaHVuYW45NjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 使用Coreinfo工具软件 (下载地址）来查看电脑是否支持Hyper-V

可以使用Coreinfo工具软件 ([下载地址](https://download.sysinternals.com/files/Coreinfo.zip)）来查看电脑是否支持Hyper-V，这是微软SysinternalsSuite工具软件套件中的一个，很实用。具体使用方法，把下载好的Coreinfo解压到桌面上，用管理员模式打开PowerShell，输入：.\ Coreinfo.exe -v，将显示你电脑虚拟化的相关信息，当然你已经添加了Hyper-V了，就无需使用这个软件了。下图所示的内容表明笔者电脑的CPU是完全支持Hyper-V的。

![Coreinfo.exe -v](https://img.ithome.com/newsuploadfiles/2018/8/20180805_203625_289.png@wm_1,k_aW1nL3F3LnBuZw==,y_20,o_100,x_20,g_9)

#### Windows 10家庭版 能够安装HYPER-V

其中涉及一个 cmd 文件。也就是把下面的代码，保存到一个 *.cmd 文件中，比如，我保存成 `install-hyper-v.cmd` 中
```
pushd "%~dp0"
 
dir /b %SystemRoot%\servicing\Packages\*Hyper-V*.mum >hyper-v.txt
 
for /f %%i in ('findstr /i . hyper-v.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"
 
del hyper-v.txt
 
Dism /online /enable-feature /featurename:Microsoft-Hyper-V-All /LimitAccess /ALL
```

然后 打开 cmd 命令行，输入 `.\install-hyper-v.cmd` 就运行起来了。

#### 修改注册表

修改前

![Current-Version-before](https://res.cloudinary.com/dmtixvmgt/image/upload/v1550483927/20181208153527303_olaite.png)

修改后

![Current-Version-after](https://res.cloudinary.com/dmtixvmgt/image/upload/v1550483962/20181208153552632_pmcuaa.png)

### 下载安装文件【Docker for Windows Installer.exe】

https://docs.docker.com/docker-for-windows/install/


### 重新启动安装文件，完成安装，重启电脑后，托盘上出现docker图标

![docker-icon](https://res.cloudinary.com/dmtixvmgt/image/upload/v1550484157/20181208153710830_y4iwyw.png)

### docker 使用

参考 https://blog.csdn.net/hunan961/article/details/79484098

## ref
- https://blog.csdn.net/tidu2chengfo/article/details/84892915
- https://www.ithome.com/html/win10/374942.htm
- https://blog.csdn.net/hunan961/article/details/79484098
- https://www.cnblogs.com/hager/p/7477270.html