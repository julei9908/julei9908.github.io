# mysql5.x（centos7.x）

#### 卸载mariadb

```bash
rpm -qa | grep mariadb
rpm -e --nodeps mariadb-libs-5.5.56-2.el7.centos.x86_64
```

#### 安装依赖

```bash
yum -y install libaio
```

#### 安装

```bash
rpm -ivh mysql-community-common-5.7.22-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-5.7.22-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-5.7.22-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-5.7.22-1.el7.x86_64.rpm
```

#### 初始化

💡 初始化生成随机密码的root账户，随机初始密码在/var/log/mysqld.log文件中

```bash
mysqld --initialize --user=mysql
```

💡 初始化生成无密码root账户

```bash
mysqld --initialize-insecure --user=mysql
```

#### 修改/etc/my.cnf的编码

```bash
[client]
default-character-set = utf8
[mysqld]
character_set_server = utf8
port = 3030
lower_case_table_names = 1
```

#### 启动mysql

```bash
systemctl restart mysqld
```

#### 修改密码

```bash
mysql> alter user 'root'@'localhost' identified by '123456';
```

#### 配置远程访问

```bash
mysql> use mysql;
mysql> update user set host='%' where user = 'root';
mysql> flush privileges;
```

#### 创建数据库并授权用户

```bash
mysql> create database if not exists dbname default charset utf8 collate utf8_general_ci;

mysql> grant all on dbname.* to 'julei'@'%' identified by '4YDf1r93';
```

#### 创建用户

```bash
mysql> create user 'julei'@'%' identified by '4YDf1r93';
```

#### 连接错误

```bash
mysql> alter user'root'@'localhost' identified with mysql_native_password
```

#### [在线安装](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/#repo-qg-yum-fresh-install)