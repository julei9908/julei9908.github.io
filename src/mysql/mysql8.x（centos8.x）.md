# mysql8.x（centos8.x）

#### 安装

```bash
yum install @mysql:8.0 -y
```

#### 启动

```bash
systemctl start mysqld
```

#### 开机启动

```bash
systemctl enable mysqld
```

#### 安全初始化

```bash
mysql_secure_installation
```

#### 创建用户

```bash
mysql> create user 'name'@'%' identified by 'password';
```

#### 创建数据库并授权用户

```bash
mysql> create database if not exists dbname;

mysql> grant all privileges on dbname.* to 'name'@'%' with grant option;
```