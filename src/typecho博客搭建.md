# typecho博客搭建

#### nginx环境

1. 创建无登录权限用户

```bash
groupadd www
useradd -s /sbin/nologin -g www www
```

2. 安装nginx

```bash
./configure --prefix=/usr/local/nginx --user=www --group=www --with-http_stub_status_module --with-http_ssl_module --with-http_v2_module --with-http_gzip_static_module --with-http_sub_module --with-pcre=/root/pcre --with-zlib=/root/zlib --with-openssl=/root/openssl
make && make install
```

3. 常用命令

```bash
./nginx                      # 启动 Nginx
./nginx -s reload            # 重新载入配置文件
./nginx -s reopen            # 重启 Nginx
./nginx -s stop              # 停止 Nginx
./nginx -v                   # 查看nginx版本信息
./nginx -V                   # 查看nginx编译信息
```

4. 配置文件

```bash
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

#### PHP环境

1. 安装curl

```bash
yum -y install c-ares-devel # 安装依赖

tar -zxf curl-7.59.0.tar.gz # 解压curl源码包

./configure --enable-ares --without-nss --with-ssl
make && make install  # 编译安装
```

2. 安装PHP依赖

```bash
yum -y install libxml2-devel
```

3. 编译安装

```bash
./configure --enable-fpm --with-fpm-user=www --with-fpm-group=www --enable-mysqlnd --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-curl=/usr/bin --enable-mbstring
make && make install
```

4. 配置文件

```bash
cd /usr/local/etc/
cp php-fpm.conf.default php-fpm.conf
cd php-fpm.d
cp www.conf.default www.conf
```

5. 修改php-fpm.conf

💡 编辑 include=/usr/local/etc/php-fpm.d/*.conf

```bash
vim /usr/local/etc/php-fpm.conf
```

6. 启动php-fpm

```bash
cd /usr/local/sbin
./php-fpm
```