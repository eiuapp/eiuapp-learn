ubuntu 安装 nginx

## env

aliyun 上面的 nginx配置，放到本地

lan server

- os: ubuntu18.04
- ip: 192.168.168.137

## aliyun

```bash
lcnx@iZwz95dxhc92qtibd4f399Z:/alidata/server/nginx/sbin$ ./nginx -V
nginx version: nginx/1.6.0
built by gcc 4.6.3 (Ubuntu/Linaro 4.6.3-1ubuntu5) 
TLS SNI support enabled
configure arguments: --user=www --group=www --prefix=/alidata/server/nginx --with-http_stub_status_module --without-http-cache --with-http_ssl_module --with-http_gzip_static_module
lcnx@iZwz95dxhc92qtibd4f399Z:/alidata/server/nginx/sbin$ 
```

## lan server

所以，我的编译参数是

```bash
ubuntu@utuntu:~/lcnx/aliyun/env/nginx-1.16.0$ --prefix=/home/ubuntu/lcnx/aliyun/env/nginx-1.16.0 --with-http_stub_status_module --without-http-cache --with-http_ssl_module --with-http_gzip_static_module
checking for OS
 + Linux 4.15.0-50-generic x86_64
checking for C compiler ... not found

./configure: error: C compiler cc is not found
```

所以要安装一下 gcc . https://blog.csdn.net/AnneQiQi/article/details/51725658

```bash
ubuntu@utuntu:~/lcnx/aliyun/env/nginx-1.16.0$ sudo apt-get  install  build-essential 
```

然后如果有报错，参考一下 [cheatsheets](https://eiuapp.github.io/cheatsheets/)
