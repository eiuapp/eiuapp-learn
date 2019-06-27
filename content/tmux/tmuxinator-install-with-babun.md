
babun 安装 	tmuxinator

## 安装 gem

```
{ ~ } master » which gem                                                                                       ~ 1
gem not found
{ ~ } master » pact install rvm                                                                                ~ 1
Working directory is /setup
Mirror is http://mirrors.kernel.org/sourceware/cygwin/
setup.ini taken from the cache

Installing rvm
Package rvm not found or ambiguous name, exiting
{ ~ } master » which scoop                                                                                       ~
/cygdrive/c/Users/DELL/scoop/shims/scoop
{ ~ } master » scoop install rvm                                                                               ~ 1
Couldn't find manifest for 'rvm'.
{ ~ } master » pact install rvm                                                                                  ~
Working directory is /setup
Mirror is http://mirrors.kernel.org/sourceware/cygwin/
setup.ini taken from the cache

Installing rvm
Package rvm not found or ambiguous name, exiting
{ ~ } master » pact install ruby          
```

直接安装 rvm 是没有希望了。

## 安装 ruby 
```
{ ~ } master » pact install ruby   
{ ~ } master » curl -sSL https://get.rvm.io | bash -s stable                                                 ~ 127
Downloading https://github.com/rvm/rvm/archive/1.29.7.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.7/1.29.7.tar.gz.asc
GPG signature verification failed for '/cygdrive/c/Users/DELL/.rvm/archives/rvm-1.29.7.tgz' - 'https://github.com/rvm/rvm/releases/download/1.29.7/1.29.7.tar.gz.asc'! Try to install GPG v2 and then fetch the public key:

    gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

or if it fails:

    command curl -sSL https://rvm.io/mpapis.asc | gpg --import -
    command curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -

In case of further problems with validation please refer to https://rvm.io/rvm/security

{ ~ } master »  gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
{ ~ } master » curl -sSL https://get.rvm.io | bash -s stable                                                 ~ 127
Downloading https://github.com/rvm/rvm/archive/1.29.7.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.7/1.29.7.tar.gz.asc
GPG signature verification failed for '/cygdrive/c/Users/DELL/.rvm/archives/rvm-1.29.7.tgz' - 'https://github.com/rvm/rvm/releases/download/1.29.7/1.29.7.tar.gz.asc'! Try to install GPG v2 and then fetch the public key:

    gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

or if it fails:

    command curl -sSL https://rvm.io/mpapis.asc | gpg --import -
    command curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -

In case of further problems with validation please refer to https://rvm.io/rvm/security

{ ~ } master »                  
{ ~ } master » command curl -sSL https://rvm.io/mpapis.asc | gpg --import -                                  ~ 127

(23) Failed writing body
{ ~ } master »                                                                                               ~ 127
{ ~ } master »  command curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -                             ~ 127
(23) Failed writing body
{ ~ } master »        
```

到官网找找看

https://github.com/rvm/rvm/issues/3623 中说用下面

```
curl -sSL https://get.rvm.io | bash -s branch /bugfix/issue-3623-cygwin-gnupg
```
替代

```
curl -sSL https://get.rvm.io | bash -s stable           
```

还是不行。

后来发现 

`gpg --version` 都没有输出。说明我们的之前 `pact install gunpg` 并没有成功。

### 安装 gpg
那问题：
- pact 安装软件后的 目录在哪里呢？

https://www.zhihu.com/question/265291192

```
>> cd \your_path_to\.babun  #进入你win系统中安装babun的目录
>> .\update.bat             #执行更新批处理文件，更新cygwin，然后问题解决
```

之后，果然可以了！~

这时，再安装 `pact install gnupg2 `

https://github.com/rvm/rvm 安装


```
{ ~ } master » curl -sSL https://get.rvm.io | bash -s stable                                                     ~
Downloading https://github.com/rvm/rvm/archive/1.29.7.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.7/1.29.7.tar.gz.asc
gpg: WARNING: unsafe permissions on homedir '/cygdrive/c/Users/DELL/.gnupg'
gpg: keybox '/cygdrive/c/Users/DELL/.gnupg/pubring.kbx' created
gpg: Signature made Fri, Jan  4, 2019  6:01:48 AM CST
gpg:                using RSA key 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
gpg: Can't check signature: No public key
GPG signature verification failed for '/cygdrive/c/Users/DELL/.rvm/archives/rvm-1.29.7.tgz' - 'https://github.com/rvm/rvm/releases/download/1.29.7/1.29.7.tar.gz.asc'! Try to install GPG v2 and then fetch the public key:

    gpg2 --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

or if it fails:

    command curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -
    command curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import -

In case of further problems with validation please refer to https://rvm.io/rvm/security

{ ~ } master »  gpg2 --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
gpg: WARNING: unsafe permissions on homedir '/cygdrive/c/Users/DELL/.gnupg'
gpg: keyserver receive failed: No such file or directory
{ ~ } master » command curl -sSL https://rvm.io/mpapis.asc | gpg2 --import -                                   ~ 2

gpg: WARNING: unsafe permissions on homedir '/cygdrive/c/Users/DELL/.gnupg'
gpg: key 3804BB82D39DC0E3: 47 signatures not checked due to missing keys
gpg: /cygdrive/c/Users/DELL/.gnupg/trustdb.gpg: trustdb created
gpg: key 3804BB82D39DC0E3: public key "Michal Papis (RVM signing) <mpapis@gmail.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1
gpg: no ultimately trusted keys found
{ ~ } master » 
```

还是报错了.........

```
{ ~ } master » command curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import -                               ~ 2

gpg: WARNING: unsafe permissions on homedir '/cygdrive/c/Users/DELL/.gnupg'
gpg: key 105BD0E739499BDB: public key "Piotr Kuczynski <piotr.kuczynski@gmail.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1
{ ~ } master » 
```


`command curl -sSL https://rvm.io/pkuczynski.asc | gpg2 --import -  ` 这个没有报错了，看来这个提示有用了！~ 

### rvm 安装


```
{ ~ } master »  curl -sSL https://get.rvm.io | bash -s stable                                                ~ 130
Downloading https://github.com/rvm/rvm/archive/1.29.7.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.7/1.29.7.tar.gz.asc
gpg: WARNING: unsafe permissions on homedir '/cygdrive/c/Users/DELL/.gnupg'
gpg: Signature made Fri, Jan  4, 2019  6:01:48 AM CST
gpg:                using RSA key 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
gpg: Good signature from "Piotr Kuczynski <piotr.kuczynski@gmail.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 7D2B AF1C F37B 13E2 069D  6956 105B D0E7 3949 9BDB
GPG verified '/cygdrive/c/Users/DELL/.rvm/archives/rvm-1.29.7.tgz'
Installing RVM to /cygdrive/c/Users/DELL/.rvm/
    Adding rvm PATH line to /cygdrive/c/Users/DELL/.profile /cygdrive/c/Users/DELL/.mkshrc /cygdrive/c/Users/DELL/.bashrc /cygdrive/c/Users/DELL/.zshrc.
    Adding rvm loading line to /cygdrive/c/Users/DELL/.profile /cygdrive/c/Users/DELL/.bash_profile /cygdrive/c/Users/DELL/.zlogin.
Installation of RVM in /cygdrive/c/Users/DELL/.rvm/ is almost complete:

  * To start using RVM you need to run `source /cygdrive/c/Users/DELL/.rvm/scripts/rvm`
    in all your open shell windows, in rare cases you need to reopen all shell windows.
{ ~ } master »                                 
```

### 启用并检查 rvm version

```
{ ~ } master » source /cygdrive/c/Users/DELL/.rvm/scripts/rvm                                                                      ~ 130
{ ~ } master » 
{ ~ } master » rvm version                                                                                                             ~
rvm 1.29.7 (latest) by Michal Papis, Piotr Kuczynski, Wayne E. Seguin [https://rvm.io]
{ ~ } master »  
```

### rvm 安装 ruby

```
{ ~ } master » rvm install 2.3.1                                                                                                                                                                                                     ~ 130
Searching for binary rubies, this might take some time.
No binary rubies available for: cygwin/unknown/i386/ruby-2.3.1.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for cygwin.
Installing requirements for cygwin.
Updating system - please wait
Installing required packages: libcrypt-devel, libffi-devel - please wait
Error running 'requirements_cygwin_libs_install libcrypt-devel libffi-devel',
please read /cygdrive/c/Users/DELL/.rvm/log/1551261019_ruby-2.3.1/package_install_libcrypt-devel_libffi-devel.log
Requirements installation failed with status: 127.
{ ~ } master »     
```

根据提示：

```
pact install libcrypt-devel libffi-devel
rvm install 2.3.1
```

还是报错了，如下：

```
{ ~ } master » rvm install 2.3.1                                                                                                   ~ 130
ruby-2.3.1 - #removing src/ruby-2.3.1 - please wait
Searching for binary rubies, this might take some time.
No binary rubies available for: cygwin/unknown/i386/ruby-2.3.1.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for cygwin.
Requirements installation successful.
__rvm_install_source:source:2: no such file or directory: /cygdrive/c/Users/DELL/.rvm/scripts/functions/manage/install/cygwin
Installing Ruby from source to: /cygdrive/c/Users/DELL/.rvm/rubies/ruby-2.3.1, this may take a while depending on your cpu(s)...
ruby-2.3.1 - #downloading ruby-2.3.1, this may take a while depending on your connection...
ruby-2.3.1 - #extracting ruby-2.3.1 to /cygdrive/c/Users/DELL/.rvm/src/ruby-2.3.1 - please wait
ruby-2.3.1 - #applying patch /cygdrive/c/Users/DELL/.rvm/patches/ruby/ruby_2_3_gcc7.patch - please wait
ruby-2.3.1 - #applying patch /cygdrive/c/Users/DELL/.rvm/patches/ruby/2.3.1/random_c_using_NR_prefix.patch - please wait
ruby-2.3.1 - #applying patch /cygdrive/c/Users/DELL/.rvm/patches/ruby/2.3.1/fix_resolv_kernel32_dll.patch - please wait
ruby-2.3.1 - #configuring - please wait
ruby-2.3.1 - #post-configuration - please wait
ruby-2.3.1 - #compiling - please wait
Error running '__rvm_make -j6',
please read /cygdrive/c/Users/DELL/.rvm/log/1551316037_ruby-2.3.1/make.log

There has been an error while running make. Halting the installation.
{ ~ } master » cat /cygdrive/c/Users/DELL/.rvm/log/1551316037_ruby-2.3.1/make.log                                                  ~ 127
+__rvm_make:0> make -j6
C:/Users/DELL/.babun/cygwin/bin/make.exe: error while loading shared libraries: ?: cannot open shared object file: No such file or directory
+__rvm_make:0> return 127
{ ~ } master »         
```


说明安装不了 2.3.1 ，文章说得没错。

之后分别尝试 ruby 1.7.0, 1.9.3, 2.5.0 都失败了，且报类似错误。

### 通过原生安装

既然babun 中的 rvm 安装 ruby 失败，那就通过原生安装

参考[这里](https://numediaweb.com/ruby-and-rubygems-in-windows-vs-babun/1288)

去 https://rubyinstaller.org/downloads/ 下载了 Ruby+Devkit 2.6.1-1 (x64) 版本，安装 到 D:\Ruby26-x64\

添加环境变量`D:\Ruby26-x64\bin`


在 cmd 中 
```
C:\Users\DELL>ruby -v
ruby 2.6.1p33 (2019-01-30 revision 66950) [x64-mingw32]

C:\Users\DELL>gem -v
3.0.1
```

但是这时，在 git-bash, babun/zsh 中还是没有找到 ruby 命令。

### 添加环境变量至 babun/zsh 环境

#### 修改 .zshrc 文件

* 添加 `export PATH=$PATH:/d/Ruby26-x64/bin`
* 添加 `alias gem='D:/\Ruby26-x64/\bin/\gem'`


参考[这里](https://www.question-defense.com/2009/03/27/rubyexe-no-such-file-or-directory-cygdrivecrubybingem-loaderror)
如果不做第2步，就会报(LoadError),如下：

```
{ ~ } master » ruby -v                                                                                                                 ~
ruby 2.6.1p33 (2019-01-30 revision 66950) [x64-mingw32]
{ tmp } master » which gem                                                                                                 ~/Desktop/tmp
/d/Ruby26-x64/bin//gem
{ tmp } master » gem -v                                                                                                    ~/Desktop/tmp
D:\Ruby26-x64\bin\ruby.exe: No such file or directory -- /d/Ruby26-x64/bin//gem (LoadError)
{ ~ } master » alias gem='D:/\Ruby26-x64/\bin/\gem'                                                                                  ~ 1
{ ~ } master » gem -v                                                                                                                  ~
3.0.1
{ ~ } master »         
```

### 安装 tmuxinator 依赖

#### （可忽略）安装 弯路
参考 http://wiki.k-zone.cn/post/wiki/tmux-pei-zhi 

```
{ tmp } master » curl -O https://rubygems.org/downloads/erubis-2.7.0.gem                                                                                                                                                     ~/Desktop/tmp
{ tmp } master » curl -O https://rubygems.org/downloads/tmuxinator-0.6.10.gem                                                                                                                                                ~/Desktop/tmp
{ tmp } master » curl -O https://rubygems.org/downloads/teamocil-1.2.gem                                                                                                                                                     ~/Desktop/tmp
{ tmp } master » gem install --local ./erubis-2.7.0.gem                                                                                                                                                                      ~/Desktop/tmp
```

在继续前，想安装 tmuxinator 的最新版本。

所以 根据上面这个链接，打开 `https://rubygems.org/` ,输入 tmuxinator ,发现最新版是 0.15.0 , 下载了

```
{ tmp } master » curl -O https://rubygems.org/downloads/tmuxinator-0.15.0.gem                                              ~/Desktop/tmp
{ tmp } master » gem install --local ./tmuxinator-0.15.0.gem                                                                ~/Desktop/tmp
ERROR:  While executing gem ... (Errno::EACCES)
    Permission denied @ rb_file_s_symlink - (completion/tmuxinator.fish, D:/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion/mux.fish)
```
报错，那就sudo 执行。
```
{ tmp } master » sudo gem install --local ./tmuxinator-0.15.0.gem                                                         ~/Desktop/tmp 1
D:\Ruby26-x64\bin\ruby.exe: No such file or directory -- /d/Ruby26-x64/bin/gem (LoadError)
{ tmp } master »  sudo D:/\Ruby26-x64/\bin/\gem install  --local ./tmuxinator-0.15.0.gem                                                                                                                                   ~/Desktop/tmp 1
ERROR:  While executing gem ... (Errno::EACCES)
    Permission denied @ rb_file_s_symlink - (completion/tmuxinator.fish, D:/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion/mux.fish)
{ tmp } master » gem install --local ./tmuxinator-0.15.0.gem                                                                                                                                                               ~/Desktop/tmp 1
ERROR:  While executing gem ... (Errno::EACCES)
    Permission denied @ rb_file_s_symlink - (completion/tmuxinator.fish, D:/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion/mux.fish)
{ tmp } master » 
```

然后就想，是不是因为文件没有下载，那手工传一下文件试试
```
{ tmp } master » ruby -v                                                                                                                                                                                                   ~/Desktop/tmp 1
ruby 2.6.1p33 (2019-01-30 revision 66950) [x64-mingw32]
{ tmp } master » ls /d/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion                                                                                                                                      ~/Desktop/tmp
{ tmp } master » ls /f/tom/dotfiles-windows/.dotfiles-private-windows-jinweilai/.tmuxinator/completion                                                                                                                   ~/Desktop/tmp 127
mux.fish  tmuxinator.bash  tmuxinator.fish  tmuxinator.zsh
{ tmp } master » ls /f/tom/dotfiles-windows/.dotfiles-private-windows-jinweilai/.tmuxinator/completion/*sh                                                                                                                   ~/Desktop/tmp
/f/tom/dotfiles-windows/.dotfiles-private-windows-jinweilai/.tmuxinator/completion/mux.fish         /f/tom/dotfiles-windows/.dotfiles-private-windows-jinweilai/.tmuxinator/completion/tmuxinator.fish
/f/tom/dotfiles-windows/.dotfiles-private-windows-jinweilai/.tmuxinator/completion/tmuxinator.bash  /f/tom/dotfiles-windows/.dotfiles-private-windows-jinweilai/.tmuxinator/completion/tmuxinator.zsh
{ tmp } master » cp /f/tom/dotfiles-windows/.dotfiles-private-windows-jinweilai/.tmuxinator/completion/*sh /d/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion                                               ~/Desktop/tmp
{ tmp } master » ls /d/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion                                                                                                                                      ~/Desktop/tmp
mux.fish  tmuxinator.bash  tmuxinator.fish  tmuxinator.zsh
{ tmp } master »  gem install --local ./tmuxinator-0.15.0.gem                                                                                                                                                                ~/Desktop/tmp
ERROR:  While executing gem ... (Errno::EACCES)
    Permission denied @ rb_file_s_symlink - (completion/tmuxinator.fish, D:/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion/mux.fish)
{ tmp } master »  ls /d/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion                                                                                                                                     ~/Desktop/tmp
{ tmp } master » cp /f/tom/dotfiles-windows/.dotfiles-private-windows-jinweilai/.tmuxinator/completion/*sh /d/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion                                               ~/Desktop/tmp
{ tmp } master » ls /d/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion                                                                                                                                      ~/Desktop/tmp
mux.fish  tmuxinator.bash  tmuxinator.fish  tmuxinator.zsh
{ tmp } master » sudo gem install --local ./tmuxinator-0.15.0.gem                                                                                                                                                            ~/Desktop/tmp
D:\Ruby26-x64\bin\ruby.exe: No such file or directory -- /d/Ruby26-x64/bin/gem (LoadError)
{ tmp } master » ls /d/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion                                                                                                                                    ~/Desktop/tmp 1
mux.fish  tmuxinator.bash  tmuxinator.fish  tmuxinator.zsh
{ tmp } master » sudo D:/\Ruby26-x64/\bin/\gem install  --local ./tmuxinator-0.15.0.gem                                                                                                                                      ~/Desktop/tmp
ERROR:  While executing gem ... (Errno::EACCES)
    Permission denied @ rb_file_s_symlink - (completion/tmuxinator.fish, D:/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion/mux.fish)
{ tmp } master » ls /d/Ruby26-x64/lib/ruby/gems/2.6.0/gems/tmuxinator-0.15.0/completion                                                                                                                                    ~/Desktop/tmp 1
{ tmp } master » which gem                                                                                                                                                                                                   ~/Desktop/tmp
gem: aliased to D:/\Ruby26-x64/\bin/\gem
{ tmp } master »      
```

通过上面看，应该就不是这个 mux.fish 文件的问题。那就再 google `Permission denied @ rb_file_s_symlink`, 找到

https://stackoverflow.com/questions/45070484/error-installing-gem-thinreports-rails-on-windows-10


要用 管理员权限 打开 命令行。

### 管理员权限 命令行安装

那就试一下，直接安装，而不是下载*.gem文件安装。

```
{ ~ } master » gem install tmuxinator                                                                                                  ~

    __________________________________________________________
    ..........................................................

    Thank you for installing tmuxinator.

    Make sure that you've set these variables in your ENV:

      $EDITOR, $SHELL

    You can run `tmuxinator doctor` to make sure everything is set.
    Happy tmuxing with tmuxinator!

    ..........................................................
    __________________________________________________________

Successfully installed tmuxinator-0.15.0
Parsing documentation for tmuxinator-0.15.0
Installing ri documentation for tmuxinator-0.15.0
Done installing documentation for tmuxinator after 0 seconds
1 gem installed
{ ~ } master »                                                                                                                         ~
```

哈哈，成功了！~

### 安装检查

```
{ ~ } master » which tmuxinator                                                                                                      ~ 1
/cygdrive/d/Ruby26-x64/bin/tmuxinator
{ ~ } master » tmuxinator ls                                                                                                    ~
D:\Ruby26-x64\bin\ruby.exe: No such file or directory -- /cygdrive/d/Ruby26-x64/bin/tmuxinator (LoadError)
{ ~ } master » which gem                                                                                                               ~
gem: aliased to D:/\Ruby26-x64/\bin/\gem
{ ~ } master » alias ruby='D:/\Ruby26-x64/\bin/\ruby.exe'                                                                              ~
{ ~ } master » which ruby                                                                                                              ~
ruby: aliased to D:/\Ruby26-x64/\bin/\ruby.exe
{ ~ } master » ruby -v                                                                                                               ~ 1
ruby 2.6.1p33 (2019-01-30 revision 66950) [x64-mingw32]
{ ~ } master » tmuxinator --version                                                                                                    ~
D:\Ruby26-x64\bin\ruby.exe: No such file or directory -- /cygdrive/d/Ruby26-x64/bin/tmuxinator (LoadError)
{ ~ } master »                        
{ ~ } master » D:/\Ruby26-x64/\bin/\tmuxinator ls                                                                                  ~ 1
tmuxinator projects:
{ ~ } master » 
```

这样的话，只能往 .zshrc.local 文件添加了

```
{ ~ } master » cat .zshrc.local                                                                                                      ~

## ruby and gem
export PATH=$PATH:/d/Ruby26-x64/bin
alias ruby='D:/\Ruby26-x64/\bin/\ruby.exe'
alias gem='D:/\Ruby26-x64/\bin/\gem'
alias tmuxinator='D:/\Ruby26-x64/\bin/\tmuxinator'

## tmuxinator
source $HOME/.tmuxinator/completion/tmuxinator.zsh
export EDITOR='vi'
{ ~ } master »       
```

是不是意味着，每一个 gem 安装的包，想要正常执行，都需要在 .zshrc.local 中添加呢？还有没有更好的解决方案呢？目前不知道。

至此，我们的tmuxinator 环境安装完毕了。

### 使用中报错

```
C:\Users\DELL>tmuxinator edit ceph-pai
Checking if tmux is installed ==> 系统找不到指定的路径。
No
Checking if $EDITOR is set ==> No
Checking if $SHELL is set ==> No

C:\Users\DELL>tmux version
'tmux' 不是内部或外部命令，也不是可运行的程序
或批处理文件。

C:\Users\DELL>
```

也就是说，使用中，tmuxinator 要去找 tmux ，找不到。

为什么呢？因为 tmux 是由  `pact install tmux` 安装的。pact 安装后的软件包，没有被 tmuxinator 识别。

那现在就尴尬了。

- pact 安装了 tmux , 安装不了tmuxinator 
- pact 安装了 gem ，gem 安装不了 ./tmuxinator-0.15.0.gem 文件
- 原生安装了ruby,gem, 安装了 tmuxinator 但是，又识别不到 tmux 

基于目前这个情况，暂时放弃了。准备在windows10中使用 WSL 然后在 WSL中的ubuntu安装tmuxinator并使用。
如果您有更好的在 win10 下使用tmuxinator的方式，请让我受教吧！~

### (可忽略) 为了安装 可用的 gem 


#### `.rbenv/bin/rbenv install 2.6.1`， 失败

```
{ ~ } master » .rbenv/bin/rbenv install 2.6.1                                                                                                               ~
C:/Users/DELL/.babun/cygwin/bin/perl.exe: error while loading shared libraries: ?: cannot open shared object file: No such file or directory
Downloading ruby-2.6.1.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.1.tar.bz2
Installing ruby-2.6.1...

BUILD FAILED (CYGWIN_NT-10.0-WOW 3.0.1(0.338/5/3) using ruby-build 20190130-4-g0e33b11)

Inspect or clean up the working tree at /tmp/ruby-build.20190301170457.21293
Results logged to /tmp/ruby-build.20190301170457.21293.log

Last 10 log lines:
                          -Wno-tautological-compare -Wno-unused-parameter \
                          -Wno-unused-value -Wsuggest-attribute=format \
                          -Wsuggest-attribute=noreturn -Wunused-variable
   * strip command:       strip
   * install doc:         yes
   * JIT support:         yes
   * man page type:       doc

---
C:/Users/DELL/.babun/cygwin/bin/make.exe: error while loading shared libraries: ?: cannot open shared object file: No such file or directory
{ ~ } master »         
```

#### 源码编译安装，失败

```
{ ruby-2.5.3 } master » ./configure
{ ruby-2.5.3 } master » make                                                                                                        ~/.rvm/src/ruby-2.5.3 127
C:/Users/DELL/.babun/cygwin/bin/make.exe: error while loading shared libraries: ?: cannot open shared object file: No such file or directory
{ ruby-2.5.3 } master » 
```

https://raw.githubusercontent.com/wzhishen/dotfiles/master/tools/install.sh

#### 通过 apt-cyg 来安装 ruby 和 gem， 失败

还是失败了。。。



## ref
- http://gvlnetworks.com/Babun-Tmux/
- http://www.elonwoo.com/2016/08/17/babun-ruby-gem-tmuxinator/
- http://wiki.k-zone.cn/post/wiki/babun-pei-zhi
- https://www.question-defense.com/2009/03/27/rubyexe-no-such-file-or-directory-cygdrivecrubybingem-loaderror
- https://numediaweb.com/ruby-and-rubygems-in-windows-vs-babun/1288
- https://stackoverflow.com/questions/45070484/error-installing-gem-thinreports-rails-on-windows-10
