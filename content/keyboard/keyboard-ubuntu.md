---
title: "Keyboard Ubuntu"
date: 2018-08-08T00:29:31+08:00
draft: false
tags: ["keyboard", "ubuntu", "xmodmap"]
categories: ["ubuntu"]
subtitle: "通过xmodmap修改ubuntu系统下的键盘布局"
descriptions: ""
bigimg:
---

通过xmodmap修改ubuntu系统下的键盘布局

因为某种需求，需要把 右ctrl与右alt 互换

## Env

- os: ubuntu desktop 16.04 
- target: swap Right Ctrl with Right Alt on my keyboard

### xmodmap

#### Installation

```
tom@tom:~$ cat ~/.Xmodmap 
clear control
clear mod1
keycode 105 = Alt_R Meta_R
keycode 108 = Control_R
add control = Control_L Control_R
add mod1 = Alt_R Meta_R
tom@tom:~$ 
tom@tom:~$ xmodmap ~/.Xmodmap
tom@tom:~$ 
```
这样，成功了。

然后放到.bashrc中，这个文件每次启动shell都会执行，但结果确是：每启动一次shell,Ctrl和CapLock就交换一次，意味着我不知道下一时刻,哪个是Ctrl键，哪个是CapLock键。

最后发现，在~目录添加一个.bash_profile文件，并将xmodmap脚本的执行放入其中。问题就解决了。

```
tom@tom:~$ cat .bash_profile 
xmodmap ~/.Xmodmap
tom@tom:~$ source .bash_profile
```
### gnome-tweak-tool(不成功)

gnome-tweak-tool 可以方便地修改某些键盘，如：互换 左ctrl与CapsLock

安装gnome-tweak-tool：

```
sudo apt install gnome-tweak-tool -y
```
运行

```
gnome-tweak-tool
```

定位到`Typing` - `Ctrl key position`，选中`Swap Ctrl and Caps Lock`。关闭窗口，然后测试去吧。

但是，我们选择`Right Alt as Right Ctrl`, 没有生效。哭。

### XKB(未测试))

Ubuntu 14.04 下通过 XKB 修改键盘映射, 实现自定义按键

XKB : 全称 X Keyboard Extension, 是 Liunx 下管理键盘输入的一套较为复杂的系统. Ubuntu 14.04 采用这套系统来支持图形界面下的键盘管理.

https://github.com/Chunlin-Li/Chunlin-Li.github.io/blob/master/blogs/linux/ubuntu-xkb-keyboard-remap.md

## Ref

- https://askubuntu.com/questions/93624/how-do-i-swap-left-ctrl-with-left-alt-on-my-keyboard
- https://www.cnblogs.com/billin/archive/2011/12/22/2298376.html
- https://www.jianshu.com/p/9b08b032a222
- https://traceflight.github.io/tech/modify-caps-lock-to-ctrl.html
- [xmodmap使用手册(中文版)](http://manpages.ubuntu.com/manpages/xenial/zh_CN/man1/xmodmap.1.html)
- [在ubuntu和mac间无缝切换](https://my.oschina.net/uniquejava/blog/844331)
- https://github.com/Chunlin-Li/Chunlin-Li.github.io/blob/master/blogs/linux/ubuntu-xkb-keyboard-remap.md