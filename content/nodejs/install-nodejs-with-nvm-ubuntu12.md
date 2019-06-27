
使用nvm安装nodejs 在 ubuntu12 下

```bash
lcnx@iZwz95dxhc92qtibd4f399Z:~/src/nginx-1.15.12$ nvm install stable
v12.0.0 is already installed.
node: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.18' not found (required by node)
node: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.17' not found (required by node)
node: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.16' not found (required by node)
nvm is not compatible with the npm config "prefix" option: currently set to ""
Run `nvm use --delete-prefix v12.0.0` to unset it.
Creating default alias: default -> stable (-> v12.0.0)
lcnx@iZwz95dxhc92qtibd4f399Z:~/src/nginx-1.15.12$ nvm use stable
node: /usr/lib/x86_64-linux-gnu/libstdc++.so.6: version `GLIBCXX_3.4.18' not found (required by node)
node: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.17' not found (required by node)
node: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.16' not found (required by node)
nvm is not compatible with the npm config "prefix" option: currently set to ""
Run `nvm use --delete-prefix v12.0.0` to unset it.
lcnx@iZwz95dxhc92qtibd4f399Z:~/src/nginx-1.15.12$ ls
auto  CHANGES  CHANGES.ru  conf  configure  contrib  html  LICENSE  Makefile  man  objs  README  src
lcnx@iZwz95dxhc92qtibd4f399Z:~/src/nginx-1.15.12$ 
```

### 安装gcc

https://blog.csdn.net/jom_ch/article/details/78738824



```bash
wget ftp://ftp.mirrorservice.org/sites/sourceware.org/pub/gcc/releases/gcc-5.5.0/gcc-5.5.0.tar.gz
tar -zxvf gcc-5.5.0.tar.gz
cd gcc-5.5.0
sudo ./configure --enable-checking=release --enable-languages=c,c++ --disable-multilib
```

报错

`configure: error: Building GCC requires GMP 4.2+, MPFR 2.4.0+ and MPC 0.8.0+.`

说明, 这个方式, 还是有问题呀.

### 升级gcc


https://itbilu.com/linux/management/NymXRUieg.html

找到

https://itbilu.com/linux/management/V1vdnt9ll.html

