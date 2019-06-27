

spacemacs 在ubuntu下的配置思考
 

## 私人配置

### 环境1 

os: wsl-ubuntu16
emacs: 26.2
spacemacs: 


### 环境2 

操作系统: Ubuntu 16.04 TLS
* Emacs版本: Emacs24.5.1

Emacs版本: Emacs25.2.2

## 问题

1. 启动后，j,k 上下移动行的时候，行会移动

-----------------------------------------------------------------------

1. emacs 高手配置


2. spacemacs 高手配置



3. spacemacs ubuntu 配置
https://github.com/lsytj0413/spacemacs-private



4. spacemacs 与 docker
https://github.com/JAremko/spacemacs

sudo docker run -it --rm -v $('pwd'):/mnt/workspace \
 -v /etc/localtime:/etc/localtime:ro \
 -v ~/.ssh/id_rsa:${UHOME}/.ssh/id_rsa:ro \
 -v ~/.gnupg:${UHOME}/.gnupg \
 -v /var/run/dbus/system_bus_socket:/var/run/dbus/system_bus_socket \
 -v /tmp/.X11-unix:/tmp/.X11-unix \
 -v /etc/machine-id:/etc/machine-id:ro \
 -e DISPLAY=$DISPLAY \
 -e TZ=UA \
 --name spacemacs jare/spacemacs


5. spacemacs 与 box

https://github.com/abellmann/spacemacs-devbox

6. spacemacs 与 dotfiles
https://github.com/mazinbokhari/dotfiles






-------------------------------------------------

配置中遇到的几个问题

google-c-style 与 c-c++ 相关

https://github.com/syl20bnr/spacemacs/pull/10190


