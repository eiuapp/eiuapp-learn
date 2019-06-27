

本文主要参考： [如何搭建优雅的windows开发环境](http://finalhome.org/%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7/%E5%A6%82%E4%BD%95%E6%90%AD%E5%BB%BA%E4%BC%98%E9%9B%85%E7%9A%84windows%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/) 。有修改。

### 如何搭建优雅的windows开发环境

[2016-12-17]()

之前写过 <如何搭建优雅的开发环境> 系列(未完结)， 分章节的介绍了开发过程中使用的工具和技巧。里面也提及了 Windows 下的工具，但是不够系统，我还是很想整理一篇讲解 windows 下具体设置的文章。

[](#命令行 "命令行")命令行
=================

[](#PowerShell "PowerShell")PowerShell
--------------------------------------

微软以后肯定是要用 PowerShell 替代 cmd.exe 的，所以早晚我们都会用到，就别停留在 cmd 下了，那个难易操作的终端环境，反正我是打开一次就不想再看到。如果你觉得 cmd.exe 也无所谓，那起码安装一个 [clink](https://mridgers.github.io/clink/) 提升一些编辑操作。喜欢 linux 命令的朋友，还可以安装下 [gow](https://github.com/bmatzelle/gow) 体验 linux 命令。

但是我还是建议大家使用 PowerShell, 特别是配合 cmder(下面会提到)。可以安装一些插件来提升使用的体验。

因为 [PowerShell](https://github.com/powershell/powershell) 也更新了好几个版本，我们可以先看下如何安装or更新 PowerShell:

*   [How to Install](https://github.com/PowerShell/PowerShell/blob/master/docs/installation/windows.md#msi)
*   [How to Install Windows PowerShell 4.0](http://social.technet.microsoft.com/wiki/contents/articles/21016.how-to-install-windows-powershell-4-0.aspx)

安装完成，我们使用 `$PSVersionTable.PSVersion` 查看下安装的版本。

关于教程，可以看看:

*   [Learning-powershell](https://github.com/PowerShell/PowerShell/tree/master/docs/learning-powershell)
*   [MSDN: PowerShell](https://msdn.microsoft.com/en-us/powershell)

官方的教程，但是我也没看过，有问题就上 MSDN 或 Google 上搜一下吧。

### [](#插件安装 "插件安装")插件安装

我不确定是否内置了命令 `Install-Module`, 因为我看到了一个模块： [PsGet](http://psget.net/), 也是提供`install-module` 命令的, 估计是对原生的扩展吧。反正有了这个命令，我们就可以来安装模块了.

查找模块，对于的命令为 `Find-Module`。

模块安装完成后，一般会自动载入，如果有些没有自动载入，可能需要自己修改配置文件， 手动 `Import-Module`。对于安装过的模块和路径，我们可以通过 `get-Module`, `Get-Module --listAvailable` 和 `$env:PSModulePath` 查看。

### [](#推荐模块 "推荐模块")推荐模块

模块可以在 [PsGet: Directory Index](http://psget.net/directory/) 中找到，我推荐几个我安装过的:

*   [Jump-Location](https://github.com/tkellogg/Jump-Location): 必装，提供 autojump 的实现，方便通过 `j xx` 进行目录跳转，配置 `open` （alias explorer.exe）, 方便打开目录
*   [Posh-git](https://github.com/dahlbyk/posh-git/): 必装， 提升 git 命令行操作体验。
*   [PSColor](https://github.com/Davlind/PSColor): 提供输入高亮
*   [PSColors](https://github.com/ecsousa/PSColors): 提供色彩高亮提示与输出，和上面那个可能是2选一关系吧, 这个感觉高端一些
*   [TabExpansionPlusPlus](https://github.com/lzybkr/TabExpansionPlusPlus/): 提升 命令后下 tab 扩展功能。
*   [PowerLS](http://psget.net/directory/PowerLS/): 提供 `powerls` 命令，可以配置 `Set-Alias -Name ls -Value PowerLS -Option AllScope` 针对目录展示色彩高亮（其实也没太多用处）
*   [Find-String](https://github.com/drmohundro/Find-String/): 提供类似 grep 命令
*   [Posh-svn](https://github.com/imobile3/posh-svn): 提升 svn 命令行操作，但是一般 svn 都用 GUI 操作了。

[](#cmder "cmder")cmder
-----------------------

[cmder](http://cmder.net/): 是 windows 下的一个终端模拟器，它只是一个壳子，结合了 ConEum, clink, git-for-windows(可选)。提供可方便的快捷操作以及舒适的UI界面。

我一般是选择 mini 版本安装，解压缩后，打开 `cmder.exe`, 运行 `cmder /register all` 即可给右键菜单，添加上 `open cmder here` 功能。

然后再从 `Settings` 中设置默认的终端，选择 PowerShell, 这样配合起来就更加完美了。基于 cmder 的配置文件，我们放置在 `cmder/config` 中， 如果是 PowerShell 的，就添加 `user_profile.ps1` 即可。启动 cmder 会自动加载这个目录下的配置。

### 配置cmder到右键菜单报错

![cmder-register-all-error: cmder Launcher all Function: Register ShellMenu Line](https://segmentfault.com/img/bVP1wB?w=879&h=351)



- 打开安装文件夹，我的是`F:\software\cmder`
- (这步可不执行)这个地址加到系统的path环境变量里面，
- 然后右键Cmder.exe属性-兼容性-以管理员身份运行此程序，
- 再重新打开Cmder.exe（最上面边框，`New console dialog...` 选择 `{cmd::Cmder as Admin}`）
- 输入`Cmder.exe /REGISTER ALL`，不报错，就行了~

这时，在文件夹的任何位置，右键菜单都有`Cmder here`选项。

这么做的好处，你可以在任何文件夹下，快速得用上cmder。

[](#babun "babun")babun
-----------------------

如果你比较 Geek 或者需要在 windows 下进行 linux 开发，那么 [babun](https://babun.github.io/): 一个整合 Cygwin, oh-my-zsh 等许多特性的终端环境，十分值得推荐。

安装过程异常简单，解压缩执行 `install.bat` 即可，都不需要管理员权限。但是需要注意的是，如果你安装过 Emacs, 一般都会设置 `HOME` 环境变量，这时 babun 会提示你不建议这么做，不用管它，直接继续安装。安装完成，你的 `~` 目录就是 `HOME` 配置的目录，一般是 `C:/User/uesr_name`。官方FAQ也给出过指导: [How to use the Windows’ user profile directory as my home directory in babun ?](https://babun.github.io/faq.html#_how_to_use_the_windows_user_profile_directory_as_my_home_directory_in_babun)

安装完成后，桌面会有一个 babun 快捷方式， 进入后可以运行 `babun shell` 查看当前的 shell, 正常应该为 `zsh`，如果不是, 可执行 `babun shell zsh` 进行配置。

我们还会通过 `babun check` 检查下环境是否安装正常，可能网络是有问题的，就需要我们去配置 `~/.babunrc` 文件，设置对应的代理。

如果有更新，执行 `babun update` 即可。

### [](#oh-my-zsh "oh-my-zsh")oh-my-zsh

当我设置了 `HOME` 后，进入 babun 执行 `upgrade_oh_my_zsh` 会报错，提示我有没提交的内容，但是我如果没设置 `HOME` 进行安装后，是可以正常更新的。。。

算了，谁叫我用了 Emacs 了，只能再搜搜解决方案，终于找到了一个issue: [Cannot update oh-my-zsh on start](https://github.com/babun/babun/issues/211) ， 里面给了解决方案为:

*   `git config core.filemode false`
*   在 `.oh-my-zsh` 目录中，执行 `git rebase --skip`
*   运行 `upgrade_oh_my_zsh` 即可更新

### [](#oh-my-zsh-插件 "oh-my-zsh 插件")oh-my-zsh 插件

当我们可以运行 zsh 后，那么我们体验的东西又被大大的扩展了，者就是一个 linux 小世界，比如 tumx （我还没有尝试），我使用的一些插件，可以推荐一下:

*   [autojump](https://github.com/wting/autojump)
*   [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
*   [proxychains](https://github.com/haad/proxychains): 貌似不是 zsh 插件

还有 autojump colored-man zsh_reload zsh-syntax-highlighting git git-flow ruby gem python pip node npm bower

这些插件，我们都在 `~/.oh-my-zsh/custom/plugins` 目录下进行 `git clone`， 然后配置 `~/.zshrc`， 最后 `souce ~/.zshrc` 完成配置。

具体的操作，可以看 [Babun 配置](http://wiki.k-zone.cn/post/wiki/babun-pei-zhi) .

[](#包管理 "包管理")包管理
=================

[](#chocolye "chocolye")chocolye
--------------------------------

*   [chocolatey](https://chocolatey.org/): The Package manager for Windows

windows 下一直缺少一个好用的包管理工具，这个就是解决这个问题的，且支持代理配置，网络也不是太大的问题了。比如我们可以通过 choco 安装一些软件以及开发工具:

*   [Yarn](https://chocolatey.org/packages/yarn)： 可以不选择安装依赖的 node, 如果你单独安装过 node 的话
*   [CClearn](https://chocolatey.org/packages/ccleaner)

由于我也是刚刚切换到 choco，所以之前的一些软件和工具，还不是用 choco 安装，但是后面我会一直用下去，因为包管理工具，配合命令行以及脚本，真是能够让你快速恢复开发环境。

[](#开发环境 "开发环境")开发环境
====================

作为一个开发者，开发环境也是需要好好配置一些的，基础的 python, node, java 都需要安装。推荐大家看下微软的一篇文章： [Configuring your Windows development environment](https://github.com/Microsoft/nodejs-guidelines/blob/master/windows-environment.md#compiling-native-addon-modules)

[](#node "node")node
--------------------

*   [nvm-windows](https://github.com/coreybutler/nvm-windows)
*   [npm-windows-upgrade](https://www.npmjs.com/package/npm-windows-upgrade): 打算逐步用 Yarn 替代 npm 了。

[](#c "c++")c++
---------------

*   [windows-build-tools](https://github.com/felixrieseberg/windows-build-tools)

如果开发 node add-on 的话，c++ 环境就是必须的，即使不开发，我们使用 `node-gyp` 的时候，也需要安装。

[](#java "java")java
--------------------

未完待续…

## ref
- https://segmentfault.com/q/1010000009976575
- [如何搭建优雅的windows开发环境](http://finalhome.org/%E5%BC%80%E5%8F%91%E5%B7%A5%E5%85%B7/%E5%A6%82%E4%BD%95%E6%90%AD%E5%BB%BA%E4%BC%98%E9%9B%85%E7%9A%84windows%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83/)
- [Windows 系统下的开发环境搭建](https://blog.yuanbin.me/posts/2015-07/2015-07-17_22-22-00/)
- [Babun 配置](http://wiki.k-zone.cn/post/wiki/babun-pei-zhi)