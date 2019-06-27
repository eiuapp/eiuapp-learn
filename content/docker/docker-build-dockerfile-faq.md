


## error之 `/var/lib/dpkg/), are you root?`

RUN apt-get update \
    apt install curl
	
	
报错如下：

```
 ---> Running in c865716a2694
E: Could not open lock file /var/lib/apt/lists/lock - open (13: Permission denied)
E: Unable to lock directory /var/lib/apt/lists/
E: Could not open lock file /var/lib/dpkg/lock - open (13: Permission denied)
E: Unable to lock the administration directory (/var/lib/dpkg/), are you root?
```

这里已经提示没有权限了，所以这里很可能是在这步操作上，用户切换成非root用户了。

https://stackoverflow.com/questions/32576928/how-to-install-new-packages-into-non-root-docker-container


## source 命令

```
Step 12 : RUN source /usr/local/bin/virtualenvwrapper.sh
 ---> Running in c13a187261ec
/bin/sh: 1: source: not found
```

遇到source命令，可以使用下面方式

```
RUN /bin/bash -c "source /usr/local/bin/virtualenvwrapper.sh"
```

https://cloud.tencent.com/developer/ask/70733

## oh-my-zsh-install-in-docker-fail

```
RUN sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

这样安装会报下面的错

```
Password: chsh: PAM: Authentication failure
```
使用下面安装语句, 开始的时候有效，后来又失效了。
```
RUN set -uex; \
    wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh; \
    sh ./install.sh; \
    rm ./install.sh
```
在安装的时候注意安装的用户哟。
```
RUN sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"; exit 0;`
```

### 还有一种思路是下面这种，但是我没有尝试

```
sudo chsh $USER -s $(which zsh)
```




https://stackoverflow.com/questions/42262908/oh-my-zsh-install-in-docker-fail
https://github.com/robbyrussell/oh-my-zsh/issues/1224#issuecomment-31623113

## 缓存

docker的build操作，默认是基于缓存，也就是你修改Dockerfile后，build任务会快速略过你之前成功的步骤，从你修改的那一步之后的操作，都会重新运行。
如果你想每一次build都不基于之前的缓存，在build 命令加上 --no-cache=true 参数
另外可以参见：

[Dockerfile最佳实践](https://tuxknight-notes.readthedocs.io/en/latest/docker/dockerfile_best_practices_take.html)

https://tuxknight-notes.readthedocs.io/en/latest/docker/dockerfile_best_practices_take.html
https://segmentfault.com/q/1010000004301005

