---
title: typecho搭建
date: 2021-06-15 17:21:45
tags: typecho
---

##### nginx环境

###### 创建无登录权限用户

```shell
[root@localhost ~]# groupadd www
[root@localhost ~]# useradd -s /sbin/nologin -g www www
```

###### 安装nginx

```shell
[root@localhost nginx-1.13.12]# ./configure --prefix=/usr/local/nginx --user=www --group=www --with-http_stub_status_module --with-http_ssl_module --with-http_v2_module --with-http_gzip_static_module --with-http_sub_module --with-pcre=/root/pcre --with-zlib=/root/zlib --with-openssl=/root/openssl
[root@localhost nginx-1.13.12]# make && make install
```

<!-- more -->

###### 常用命令

```shell
[root@localhost sbin]# ./nginx                      # 启动 Nginx
[root@localhost sbin]# ./nginx -s reload            # 重新载入配置文件
[root@localhost sbin]# ./nginx -s reopen            # 重启 Nginx
[root@localhost sbin]# ./nginx -s stop              # 停止 Nginx
[root@localhost sbin]# ./nginx -v                   # 查看nginx版本信息
[root@localhost sbin]# ./nginx -V                   # 查看nginx编译信息    
```

###### 配置文件

```
server {
    listen          80;
    server_name     localhost;
    root            /home/www/build;
    index           index.html index.htm index.php;
    if (!-e $request_filename) {
        rewrite ^(.*)$ /index.php$1 last;
    }
    location ~ .*\.php(\/.*)*$ {
        include fastcgi.conf;
        fastcgi_pass  127.0.0.1:9000;
        set $path_info "";
        set $real_script_name $fastcgi_script_name;
        if ($fastcgi_script_name ~ "^(.+?\.php)(/.+)$") {
            set $real_script_name $1;
            set $path_info $2;
        }
        fastcgi_param SCRIPT_FILENAME $document_root$real_script_name;
        fastcgi_param SCRIPT_NAME $real_script_name;
        fastcgi_param PATH_INFO $path_info;
    }
    access_log logs/yourdomain.log combined;
}
```

##### PHP环境

###### 安装curl

- 安装依赖
  
  ```shell
  [root@localhost ~]# yum -y install c-ares-devel
  ```

- 解压curl源码包
  
  ```shell
  [root@localhost ~]#  tar -zxf curl-7.59.0.tar.gz
  ```

- 编译安装
  
  ```shell
  [root@localhost curl-7.59.0]# ./configure --enable-ares --without-nss --with-ssl
  [root@localhost curl-7.59.0]# make && make install
  ```

###### 安装PHP依赖

```shell
[root@localhost ~]# yum -y install libxml2-devel
```

###### 编译安装

```shell
[root@localhost php-7.2.4]# ./configure --enable-fpm --with-fpm-user=www --with-fpm-group=www --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-curl=/usr/bin --enable-mbstring
[root@localhost php-7.2.4]# make && make install
```

###### 配置文件

```shell
[root@localhost ~]# cd /usr/local/etc/
[root@localhost etc]# cp php-fpm.conf.default php-fpm.conf
[root@localhost etc]# cd php-fpm.d
[root@localhost php-fpm.d]# cp www.conf.default www.conf
```

###### 修改php-fpm.conf

```shell
[root@localhost php-fpm.d]# vim /usr/local/etc/php-fpm.conf
编辑 include=/usr/local/etc/php-fpm.d/*.conf
```

###### 启动php-fpm

```shell
[root@localhost php-fpm.d]# cd /usr/local/sbin
[root@localhost sbin]# ./php-fpm
```
