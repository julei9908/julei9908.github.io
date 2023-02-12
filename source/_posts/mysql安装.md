---
title: mysql安装
date: 2021-06-15 17:24:27
tags: [mysql, 数据库]
---

##### centos

###### 卸载mariadb

```shell
[root@localhost ~]# rpm -qa | grep mariadb
[root@localhost ~]# rpm -e --nodeps mariadb-libs-5.5.56-2.el7.centos.x86_64
```

###### 安装依赖

```shell
[root@localhost ~]# yum -y install libaio
```

<!-- more -->

###### 安装

```shell
[root@localhost ~]# rpm -ivh mysql-community-common-5.7.22-1.el7.x86_64.rpm
[root@localhost ~]# rpm -ivh mysql-community-libs-5.7.22-1.el7.x86_64.rpm
[root@localhost ~]# rpm -ivh mysql-community-client-5.7.22-1.el7.x86_64.rpm
[root@localhost ~]# rpm -ivh mysql-community-server-5.7.22-1.el7.x86_64.rpm
```

###### 初始化

- 初始化生成随机密码的root账户，随机初始密码在/var/log/mysqld.log文件中
  
  ```shell
   [root@localhost ~]# mysqld --initialize --user=mysql
  ```

- 初始化生成无密码root账户
  
  ```shell
   [root@localhost ~]# mysqld --initialize-insecure --user=mysql
  ```

###### 修改/etc/my.cnf的编码

```
[client]
default-character-set = utf8
[mysqld]
character_set_server = utf8
port = 3030
lower_case_table_names = 1
```

###### 启动mysql

```shell
[root@localhost ~]# systemctl restart mysqld
```

###### 修改密码

```shell
mysql> alter user 'root'@'localhost' identified by '123456';
```

###### 配置远程访问

```shell
mysql> use mysql;
mysql> update user set host='%' where user = 'root';
mysql> flush privileges;
```

###### 创建数据库并授权用户

```shell
create database if not exists dbname default charset utf8 collate utf8_general_ci;

grant all on dbname.* to 'julei'@'%' identified by '4YDf1r93';
```

###### 创建用户

```shell
create user 'julei'@'%' identified by '4YDf1r93';
```

###### 备注

```shell
mysql> alter user'root'@'localhost' identified with mysql_native_password
```

[在线安装](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/#repo-qg-yum-fresh-install)

##### centos8（mysql8.0）

###### 安装

```shell
[root@localhost ~]# yum install @mysql:8.0 -y
```

###### 启动MySQL8.0服务和开机启动

```shell
[root@localhost ~]# systemctl start mysqld
[root@localhost ~]# systemctl enable mysqld
```

###### 安全初始化

```shell
[root@localhost ~]# mysql_secure_installation
```

###### 创建用户

```shell
create user 'julei'@'%' identified by '4YDf1r93';
```

###### 创建数据库并授权用户

```shell
create database if not exists dbname;

grant all privileges on dbname.* to 'julei'@'%' with grant option;
```

##### windows（mysql8.0）

###### [安装visual c++](https://support.microsoft.com/zh-cn/topic/%E6%9C%80%E6%96%B0%E6%94%AF%E6%8C%81%E7%9A%84-visual-c-%E4%B8%8B%E8%BD%BD-2647da03-1eea-4433-9aff-95f26a218cc0)

###### mysql安装目录创建my.ini配置文件

```
[mysqld]
basedir=D:\\mysql
datadir=D:\\mydata\\data
```

###### 初始化

```shell
bin\mysqld --initialize --console
```

###### 服务安装（管理员权限）

- 安装服务
  
  ```shell
    bin\mysqld --install
  ```

- 移除服务
  
  ```shell
    bin\mysqld --remove
  ```
