# svn服务搭建

#### 关闭selinux

```bash
vim /etc/selinux/config

SELINUX=disabled
```

#### 安装svn和模块

```bash
yum -y install mod_dav_svn subversion  #默认会安装apache
```

#### 创建版本仓库目录

```bash
mkdir -p /var/svn/repos/
```

#### 创建版本库

💡 指定数据存储为 FSFS，如果要指定为 Berkeley DB，则将 fsfs 替换为 bdb

```bash
svnadmin create --fs-type fsfs /var/svn/repos
```

#### 编辑apache配置文件

```bash
vim /etc/httpd/conf/httpd.conf

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

#### 创建passwd文件

```bash
htpasswd -cd /var/svn/passwd username
```

#### 创建authz文件

```bash
vim /var/svn/authz

[groups]
admin = julei
[/]
* = r
@admin = rw
```

#### 修改权限

```bash
chown -R apache:apache svn
```