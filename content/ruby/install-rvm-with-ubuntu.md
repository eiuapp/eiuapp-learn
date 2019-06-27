install rvm with ubuntu

去 https://github.com/rvm/rvm#installing-rvm 

```bash
vagrant@ubuntu-xenial:~$ curl -L https://get.rvm.io | bash -s stable
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   194  100   194    0     0     29      0  0:00:06  0:00:06 --:--:--    48
100 24168  100 24168    0     0   1908      0  0:00:12  0:00:12 --:--:--  6628
Downloading https://github.com/rvm/rvm/archive/1.29.7.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.7/1.29.7.tar.gz.asc
gpg: directory `/home/vagrant/.gnupg' created
gpg: new configuration file `/home/vagrant/.gnupg/gpg.conf' created
gpg: WARNING: options in `/home/vagrant/.gnupg/gpg.conf' are not yet active during this run
gpg: keyring `/home/vagrant/.gnupg/pubring.gpg' created
gpg: Signature made Thu 03 Jan 2019 10:01:48 PM UTC using RSA key ID 39499BDB
gpg: Can't check signature: public key not found
GPG signature verification failed for '/home/vagrant/.rvm/archives/rvm-1.29.7.tgz' - 'https://github.com/rvm/rvm/releases/download/1.29.7/1.29.7.tar.gz.asc'! Try to install GPG v2 and then fetch the public key:

    gpg --keyserver hkp://pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

or if it fails:

    command curl -sSL https://rvm.io/mpapis.asc | gpg --import -
    command curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -

In case of further problems with validation please refer to https://rvm.io/rvm/security

vagrant@ubuntu-xenial:~$ source ~/.rvm/scripts/rvm
-bash: /home/vagrant/.rvm/scripts/rvm: No such file or directory
vagrant@ubuntu-xenial:~$ ls ~/.rvm/
archives  src
vagrant@ubuntu-xenial:~$ which rvm
```

说明rvm 没有安装成功。为什么？

去 http://rvm.io/

说明了先运行 `gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB`


```bash
vagrant@ubuntu-xenial:~$ gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
The program 'gpg2' is currently not installed. To run 'gpg2' please ask your administrator to install the package 'gnupg2'
vagrant@ubuntu-xenial:~$ 
```

要先安装 gpg2

https://blog.programster.org/ubuntu-install-gpg-2

```bash
vagrant@ubuntu-xenial:~$ sudo apt-get install gnupg2 -y
```




