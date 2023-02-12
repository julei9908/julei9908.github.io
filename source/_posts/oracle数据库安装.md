---
title: oracle数据库安装
date: 2021-06-15 17:23:57
tags: [oracle, 数据库]
---

###### 安装x11

```shell
[root@localhost ~]# yum -y install xorg-x11-xauth xorg-x11-utils
```

###### 安装依赖包

```shell
[root@localhost ~]# yum -y install binutils compat-libcap1 compat-libstdc++-33 compat-libstdc++-33.i686 gcc gcc-c++ glibc glibc.i686 glibc-devel glibc-devel.i686 ksh libaio libaio.i686 libaio-devel libaio-devel.i686 libgcc libgcc.i686 libstdc++ libstdc++.i686  libstdc++-devel libstdc++-devel.i686 libXi libXi.i686 libXtst libXtst.i686 make sysstat
```

<!-- more -->

###### 创建用户和组

```shell
[root@localhost ~]# groupadd oinstall
[root@localhost ~]# groupadd dba
[root@localhost ~]# useradd -g oinstall -G dba oracle
[root@localhost ~]# passwd oracle
```

###### 创建安装目录

```shell
[root@localhost ~]# mkdir -p /u01/app/oracle/product/11.2.0/dbhome_1
[root@localhost ~]# chown -R oracle:oinstall /u01/
```

###### 配置环境变量

```shell
[oracle@localhost ~]# vim .bash_profile

export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/dbhome_1
export ORACLE_SID=orcl
export PATH=$ORACLE_HOME/bin:/usr/sbin:$PATH
```

###### 安装错误

```shell
vim $ORACLE_HOME/sysman/lib/ins_emagent.mk
Search for the line
$(MK_EMAGENT_NMECTL)
Change it to:
$(MK_EMAGENT_NMECTL) -lnnz11
```

###### 远程连接

```shell
SQL> alter user username account unlock
```

###### 注意

```shell
使用mobaxterm安装直接登录oracle账号
```
