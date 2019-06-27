
记一次nginx的漏洞补救

## env

安全报告中提示 `nginx 安全漏洞（CVE-2018-16843）` ,百度，`nginx CVE-2018-16843` 

- nginx: 
    - version: 1.13.0 
    - directory: 
         - 源码： /usr/svc/nginx-1.13.0
         - 编译后：/opt/local/nginx 
- os: solaris10

## step

### 找版本 ###

https://cloud.tencent.com/info/71abe1e1ae97740ce376d8b46f894dbd.html

提示，要升级到 1.14.1 。去 github.com 查一下 nginx的releases.

https://github.com/nginx/nginx/releases

果然，
```
on 6 Nov 2018 
release-1.14.1
```

那我们快速安装 1.14.1 或以上版本吧。

### 不编译 ###

试图躲过编译安装 

因为是 solaris10, 机器上，没有 c 编译器，所以，看能不能躲过编译安装

https://www.opencsw.org/packages/CSWnginx/


```bash
pkgadd -d http://get.opencsw.org/now
/opt/csw/bin/pkgutil -U
/opt/csw/bin/pkgutil -i nginx 
/opt/csw/bin/pkgutil -y -i nginx 
/usr/sbin/pkgchk -L CSWnginx # list files
```

/opt/csw/bin/pkgutil -i nginx 是为了查看版本号，看是不是我们想要的版本，如果不是，则不安装。

查了一下，版本是 1.13.8 不行呀。躲不过去了。老老实实去编译吧。（一般不编译的最新版本一般都会滞后,其实，心里也有预料）

### 编译 ###


https://www.nginx.com/resources/wiki/start/topics/tutorials/solaris_10_u5/

https://www.nginx.com/resources/wiki/start/topics/tutorials/solaris_11/

#### 找源码 ####

查找一下 在 1.14.1后的 releases 

https://github.com/nginx/nginx/releases?after=release-1.14.1

找到感觉合适的版本号

http://nginx.org/en/download.html

比如 http://nginx.org/download/nginx-1.14.2.tar.gz

下载吧。

```bash
gunzip nginx-1.14.2.tar.gz
tar -xvf nginx-1.14.2.tar.gz

```

#### configure（第一次） ####

##### 拿 configure 参数 #####

先要去原来的nginx 编译后文件夹找到 nginx 启动文件，输入 nginx -V 拿到 configure 的参数。

```bash
-bash-3.2# cd /opt/local/nginx/     
-bash-3.2# ./sbin/nginx -V
nginx version: nginx/1.13.0
built by Sun C 5.14 SunOS_sparc 2016/05/31
built with OpenSSL 1.1.0e  16 Feb 2017
TLS SNI support enabled
configure arguments: --prefix=/opt/local/nginx --with-cpu-opt=sparc64 --with-http_ssl_module --with-cc-opt='-I /opt/local/include' --with-ld-opt='-L /opt/local/lib -R /lib -R /usr/lib -R /opt/local/lib'
-bash-3.2# 
```

把configure 参数 里的目录，修改成我们的新nginx安装的对应目录

` ./configure  --prefix=/opt/local2/nginx --with-cpu-opt=sparc64  --with-http_ssl_module --with-cc-opt="-I /opt/local2/include" --with-ld-opt="-L /opt/local2/lib -R /lib -R /usr/lib -R /opt/local2/lib" `


##### 尝试 configure #####

```bash
-bash-3.2#  ./configure  --prefix=/opt/local2/nginx --with-cpu-opt=sparc64  --with-http_ssl_module --with-cc-opt="-I /opt/local2/include" --with-ld-opt="-L /opt/local2/lib -R /lib -R /usr/lib -R /opt/local2/lib" 
checking for OS
 + SunOS 5.10 sun4v
checking for C compiler ... not found

./configure: error: C compiler cc is not found

-bash-3.2# 
```

需要 cc 

#### solaris 找 gcc ####

```bash
-bash-3.2# pkginfo | grep -i gcc
application CSWgcc4core                      gcc4core - GNU C compiler
application CSWgcc4g++                       gcc4g++ - GNU C++ Compiler
application CSWlibgcc-s1                     libgcc_s1 - The GNU Compiler Collection, libgcc_s.so.1
application SPRO-studio-gccrt                gcc 5.1.0 libstdc++ and libgcc_s headers and libraries
system      SUNWgcc                          gcc - The GNU C compiler
system      SUNWgccruntime                   GCC Runtime libraries
-bash-3.2# which SUNWgcc
no SUNWgcc in /usr/sbin /usr/bin /opt/SUNWSpro/bin /usr/ccs/bin /usr/sfw/bin /opt/local/bin /usr/ucb
```

https://www.unix.com/solaris/165222-there-gcc-but-doesnt-work.html

```bash
-bash-3.2# which gcc
/usr/sfw/bin/gcc
-bash-3.2# /usr/sfw/bin/gcc  --version
gcc (GCC) 3.4.3 (csl-sol210-3_4-branch+sol_rpath)
Copyright (C) 2004 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

-bash-3.2# 
```

#### configure（第二次） ####

```bash
-bash-3.2# CC="/usr/sfw/bin/gcc" ./configure  --prefix=/opt/local2/nginx --with-cpu-opt=sparc64  --with-http_ssl_module --with-cc-opt="-I /opt/local2/include" --with-ld-opt="-L /opt/local2/lib -R /lib -R /usr/lib -R /opt/local2/lib" 
```


##### ./configure 示例 #####

```bash
-bash-3.2# CC="/opt/SUNWspro/bin/cc" ./configure --prefix=/opt/local2/nginx --with-cpu-opt=sparc64  --with-http_ssl_module --with-cc-opt="-I /opt/local2/include" --with-ld-opt="-L /opt/local2/lib -R /lib -R /usr/lib -R /opt/local2/lib" 
checking for OS
 + SunOS 5.10 sun4v
checking for C compiler ... found
 + using Sun C compiler
 + Sun C version: 5.14 SunOS_sparc 2016/05/31
checking for --with-ld-opt="-L /opt/local2/lib -R /lib -R /usr/lib -R /opt/local2/lib" ... found
checking for -Wl,-E switch ... not found
checking for gcc builtin atomic operations ... disabled
checking for C99 variadic macros ... found
checking for gcc variadic macros ... found
checking for gcc builtin 64 bit byteswap ... not found
checking for unistd.h ... found
checking for inttypes.h ... found
checking for limits.h ... found
checking for sys/filio.h ... found
checking for sys/param.h ... found
checking for sys/mount.h ... found
checking for sys/statvfs.h ... found
checking for crypt.h ... found
checking for SunOS specific features
checking for sendfilev() ... found
checking for event ports ... found
checking for nobody group ... found
checking for poll() ... found
checking for /dev/poll ... found
checking for kqueue ... not found
checking for crypt() ... found
checking for F_READAHEAD ... not found
checking for posix_fadvise() ... not found
checking for O_DIRECT ... not found
checking for F_NOCACHE ... not found
checking for directio() ... found
checking for statfs() ... not found
checking for statvfs() ... found
checking for dlopen() ... found
checking for sched_yield() ... not found
checking for sched_yield() in librt ... found
checking for sched_setaffinity() ... not found
checking for SO_SETFIB ... not found
checking for SO_REUSEPORT ... not found
checking for SO_ACCEPTFILTER ... not found
checking for SO_BINDANY ... not found
checking for IP_TRANSPARENT ... not found
checking for IP_BINDANY ... not found
checking for IP_BIND_ADDRESS_NO_PORT ... not found
checking for IP_RECVDSTADDR ... found
checking for IP_SENDSRCADDR ... not found
checking for IP_PKTINFO ... found
checking for IPV6_RECVPKTINFO ... found
checking for TCP_DEFER_ACCEPT ... not found
checking for TCP_KEEPIDLE ... not found
checking for TCP_FASTOPEN ... not found
checking for TCP_INFO ... not found
checking for accept4() ... not found
checking for eventfd() ... not found
checking for eventfd() (SYS_eventfd) ... not found
checking for int size ... 4 bytes
checking for long size ... 8 bytes
checking for long long size ... 8 bytes
checking for void * size ... 8 bytes
checking for uint32_t ... found
checking for uint64_t ... found
checking for sig_atomic_t ... found
checking for sig_atomic_t size ... 4 bytes
checking for socklen_t ... found
checking for in_addr_t ... found
checking for in_port_t ... found
checking for rlim_t ... found
checking for uintptr_t ... uintptr_t found
checking for system byte ordering ... big endian
checking for size_t size ... 8 bytes
checking for off_t size ... 8 bytes
checking for time_t size ... 8 bytes
checking for AF_INET6 ... found
checking for setproctitle() ... not found
checking for pread() ... found
checking for pwrite() ... found
checking for pwritev() ... not found
checking for sys_nerr ... not found
checking for _sys_nerr ... not found
checking for maximum errno ... found
checking for localtime_r() ... found
checking for clock_gettime(CLOCK_MONOTONIC) ... not found
checking for clock_gettime(CLOCK_MONOTONIC) in librt ... found
checking for posix_memalign() ... not found
checking for memalign() ... found
checking for mmap(MAP_ANON|MAP_SHARED) ... found
checking for mmap("/dev/zero", MAP_SHARED) ... found
checking for System V shared memory ... found
checking for POSIX semaphores ... not found
checking for POSIX semaphores in libpthread ... not found
checking for POSIX semaphores in librt ... found
checking for struct msghdr.msg_control ... not found
checking for ioctl(FIONBIO) ... found
checking for struct tm.tm_gmtoff ... not found
checking for struct dirent.d_namlen ... not found
checking for struct dirent.d_type ... not found
checking for sysconf(_SC_NPROCESSORS_ONLN) ... found
checking for sysconf(_SC_LEVEL1_DCACHE_LINESIZE) ... not found
checking for openat(), fstatat() ... found
checking for getaddrinfo() ... found
checking for PCRE library ... not found
checking for PCRE library in /usr/local/ ... not found
checking for PCRE library in /usr/include/pcre/ ... not found
checking for PCRE library in /usr/pkg/ ... not found
checking for PCRE library in /opt/local/ ... found
checking for PCRE JIT support ... found
checking for OpenSSL library ... not found
checking for OpenSSL library in /usr/local/ ... not found
checking for OpenSSL library in /usr/pkg/ ... not found
checking for OpenSSL library in /opt/local/ ... found
checking for zlib library ... found
creating objs/Makefile

Configuration summary
  + using system PCRE library
  + using system OpenSSL library
  + using system zlib library

  nginx path prefix: "/opt/local2/nginx"
  nginx binary file: "/opt/local2/nginx/sbin/nginx"
  nginx modules path: "/opt/local2/nginx/modules"
  nginx configuration prefix: "/opt/local2/nginx/conf"
  nginx configuration file: "/opt/local2/nginx/conf/nginx.conf"
  nginx pid file: "/opt/local2/nginx/logs/nginx.pid"
  nginx error log file: "/opt/local2/nginx/logs/error.log"
  nginx http access log file: "/opt/local2/nginx/logs/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
  nginx http uwsgi temporary files: "uwsgi_temp"
  nginx http scgi temporary files: "scgi_temp"

-bash-3.2# cd /opt/local2/
-bash-3.2# ls -a 
include/ lib/     nginx/   
-bash-3.2# ls -a nginx/
.   ..
-bash-3.2# ls -a lib/
.   ..
-bash-3.2# ls -a include/
.   ..
-bash-3.2# 
```

找到一大把地 not found, 很多 not found 不影响，不需要找到相应的软件, 并不意味 configure 失败，且，虽然文件夹中文件全无.


#### make ####

文件夹中文件, 由 make 生成.  

```bash
-bash-3.2# make && make install 
```

#### 检验


```bash
-bash-3.2# cd /opt/local2/nginx/    
-bash-3.2# ls
conf  html  logs  sbin
nginx version: nginx/1.15.12
built by gcc 3.4.3 (csl-sol210-3_4-branch+sol_rpath)
built with OpenSSL 1.1.0e  16 Feb 2017
TLS SNI support enabled
configure arguments: --prefix=/opt/local2/nginx --with-cpu-opt=sparc64 --with-http_ssl_module --with-cc-opt='-I /opt/local2/include' --with-ld-opt='-L /opt/local2/lib -R /lib -R /usr/lib -R /opt/local2/lib'
-bash-3.2# 
```




