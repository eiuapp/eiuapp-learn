
https://www.slant.co/topics/1843/~best-windows-package-managers

本文主要介绍以下工具

## package manager for Windows:

    * Chocolatey
    * Scoop
    * ninite
    * pact(in babun)

### 结论

- 请注意：win10或以上有一个 WSL。如果你支持 WSL 请使用 WSL。能在WSL完成的事情，就请不要用以下 package manager。
- 如果是babun用户，请直接使用 pack 。pack不能完成的任务，再考虑scoop，再考虑chocolatey
- scoop 和 chocolatey 将是你目前比较好的包管理工具。可以都使用
- 能用 scoop 安装的，就不要 chocolatey 安装。不能 scoop 安装的，都用 chocolatey 安装
- 两者都不能的，请安 scoop 方式定制
- 私人内容，用 scoop 定制
- scoop 安装、卸载、更新、清理 时，清保证软件未正在被使用（这一点，是致命的缺点呀，具体原因见 h404bi 的博文）


### Scoop


https://www.h404bi.com/blog/2018/05/12/talk-about-scoop-the-package-manager-for-windows-again.html

https://davidsheh.github.io/2017/09/09/windows-chocolatey-scoop/

#### scoop 个人库

https://github.com/h404bi/dorado

### chocolatey

https://chocolatey.org/

## vs

### chocolatey vs scoop

https://github.com/lukesampson/scoop/wiki/Chocolatey-Comparison
https://www.reddit.com/r/devops/comments/9o4si5/installing_dependencies_on_windows_do_you_use/
https://www.slant.co/versus/6470/6471/~chocolatey_vs_scoop

### windows vs mac

https://davidsheh.github.io/2017/09/09/windows-chocolatey-scoop/ 认为是：

对应于 Mac 下的 Homebrew) + iTerm 2 + Fish shell ， Windows 下是 Chocolatey( + Scoop) + ConEmu + PowerShell。

但是，经过时间选择：

- ConEmu 已经被 cmder 封装
- powershell 明显，不是最佳的shell工具，因为，与linux 下的shell 不是一个套路。可以试着使用 [babun](https://github.com/babun/babun), fish 代替着（我正在尝试的路上）。

为了更快切换工程，请记得使用 tmux 哟！~

因为 tmux 的原因。scoop 不支持安装 tmux，而pact 可以安装，说明 babun 这个工具，还是有优势。

babun/pact( + scoop) + cmder + babun/zsh( + fish) + tmux + tmuxinator 这个组合，可能是非 WSL 用户的最佳选择。

如果你支持 WSL 请使用 WSL，下面组合，可能是linux重度用户的最佳选择：

babun/pact( + scoop) + cmder + babun/zsh( + fish) + tmux  这个组合，嘿嘿，是我非 WSL 功能的组合。

WSL/ubuntu/apt + cmder + WSL/ubuntu/zsh( + fish) + tmux + tmuxinator 这个组合，嘿嘿，是我 WSL 功能的组合。

## 新势力

https://appget.net/