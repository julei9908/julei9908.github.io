---
title: redis安装
date: 2021-06-15 17:23:26
tags: [redis, 数据库]
---

###### 安装依赖包

```shell
[root@localhost redis-4.0.9]# yum -y install tcl
```

###### 解压源码包

```shell
[root@localhost ~]# tar -zxf redis-4.0.9.tar.gz
```

<!-- more -->

###### 编译

```shell
[root@localhost redis-4.0.9]# make
[root@localhost redis-4.0.9]# make test
```

###### 安装

```shell
[root@localhost redis-4.0.9]# make PREFIX=/usr/local/redis install
```

###### 复制配置文件

```shell
[root@localhost redis-4.0.9]# cp redis.conf  /etc/redis/6379.conf
```

###### 设置自启动

- 编辑6379.conf设置daemonize为yes

- 复制自启动脚本
  
  ```shell
   [root@localhost redis-4.0.9]# cp utils/redis_init_script  /etc/init.d/redisd
  ```

- 修改启动脚本参数
  
  ```shell
   #!/bin/sh的下方添加：chkconfig: 2345 90 10
  
   REDISPORT=6379
   EXEC=/usr/local/redis/bin/redis-server
   CLIEXEC=/usr/local/redis/bin/redis-cl
  ```

- 设置自启动
  
  ```shell
   [root@localhost init.d]# chkconfig redisd on
  ```

- 启动
  
  ```shell
   [root@localhost init.d]# systemctl start redisd
  ```
