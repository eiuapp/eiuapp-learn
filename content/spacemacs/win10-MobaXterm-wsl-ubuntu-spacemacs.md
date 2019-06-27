借助 MobaXterm 使得 wsl-ubuntu 在 win10 下可使用 spacemaces

## env

- host: 
	- os: win10
	- MobaXterm: MobaXterm_Portable_v11.1
- wsl: 
	- os: ubuntu16
	- spacemaces: 26.2.0


## step

```bash
DESKTOP-APB1HCJ% emacs --insecure
process 4212: D-Bus library appears to be incorrectly set up; failed to read machine uuid: UUID file '/etc/machine-id' should contain a hex string of length 32, not length 0, with no other text
See the manual page for dbus-uuidgen to correct this issue.
^C%
DESKTOP-APB1HCJ%
```

`sudo apt-get install dbus-x11`

没效果


https://token2shell.com/howto/x410/setting-up-wsl-for-linux-gui-apps/


`sudo dbus-uuidgen --ensure`

emacs

错误提示消失了。

发现不能正常加载 spacemaces 的配置，只有emacs的配置

然后，看到了 

https://zhuanlan.zhihu.com/p/36784160

这里面确实有一些很好的东西.

是.

最后发现,刚刚只有悬emacs而没有加载spaceamcs的原因,是没有重新安装26.2的包.

- 转移 ~/.spacemacs.d/ 
- 修改 ~/.emacs.d/core/templates/.spacemacs.template 中的 下载源(来自一个网站)
- emacs  --debug-init --insecure 会安装一些包
- 把 ~/.spacemacs.env 和 ~/.spacemacs 文件转移
- 然后把原来的 ~/.spacemacs.d/ 还原回来
- 把 刚刚的 ~/.spacemacs.env 放到 ~/.spacemacs.d 中
- emacs  --debug-init --insecure 会安装一些包

这样,就可以开心使用 spacemacs 了.




