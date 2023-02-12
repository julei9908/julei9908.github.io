---
title: svn安装
date: 2021-06-15 17:23:12
tags: svn
---

###### 关闭selinux

```shell
[root@localhost ~]# vim /etc/selinux/config

SELINUX=disabled
```

###### 安装svn和模块

```shell
[root@localhost ~]# yum -y install mod_dav_svn subversion  #默认会安装apache
```

<!-- more -->

###### 创建版本仓库目录

```shell
[root@localhost ~]# mkdir -p /var/svn/repos/
```

###### 创建版本库

```shell
指定数据存储为 FSFS，如果要指定为 Berkeley DB，则将 fsfs 替换为 bdb
[root@localhost ~]# svnadmin create --fs-type fsfs /var/svn/repos
```

###### 编辑apache配置文件

```shell
[root@localhost ~]# vim /etc/httpd/conf/httpd.conf

<Location /repos>
  DAV svn
  SVNPath /var/svn/repos
  AuthType Basic
  AuthName "svn repos"
  AuthUserFile /var/svn/passwd
  AuthzSVNAccessFile /var/svn/authz
  Require valid-user
</Location>
```

###### 创建passwd文件

```shell
[root@localhost ~]# htpasswd -cd /var/svn/passwd julei
```

###### 创建authz文件

```shell
[root@localhost ~]# vim /var/svn/authz

[groups]
admin = julei
[/]
* = r
@admin = rw
```

###### 修改权限

```shell
[root@localhost var]# chown -R apache:apache svn
```
