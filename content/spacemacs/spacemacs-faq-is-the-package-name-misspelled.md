
spacemacs

## 安装 Package 失败： Package is unavailable. Is the package name misspelled ?

https://emacs-china.org/t/topic/2662/13

我昨天遇到同样问题，参照 这个 issue 24 解决了。方法是打开配置文件（SPC f e d）找到 dotspacemacs-elpa-timeout 5 这一行改成 dotspacemacs-elpa-timeout 300 （或任何比较大的数），重启。不知道能不能帮到你。


## nyan-mode Invalid image type 'xpm'

