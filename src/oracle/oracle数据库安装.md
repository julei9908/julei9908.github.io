# oracle数据库安装

💡 使用oracle账号登录安装

#### 安装x11

```bash
yum -y install xorg-x11-xauth xorg-x11-utils
```

#### 安装依赖包

```bash
yum -y install binutils compat-libcap1 compat-libstdc++-33 compat-libstdc++-33.i686 gcc gcc-c++ glibc glibc.i686 glibc-devel glibc-devel.i686 ksh libaio libaio.i686 libaio-devel libaio-devel.i686 libgcc libgcc.i686 libstdc++ libstdc++.i686  libstdc++-devel libstdc++-devel.i686 libXi libXi.i686 libXtst libXtst.i686 make sysstat
```

#### 创建用户和组

```bash
groupadd oinstall
groupadd dba
useradd -g oinstall -G dba oracle
passwd oracle
```

#### 创建安装目录

```bash
mkdir -p /u01/app/oracle/product/11.2.0/dbhome_1
chown -R oracle:oinstall /u01/
```

#### 配置环境变量

```bash
vim .bash_profile

export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/dbhome_1
export ORACLE_SID=orcl
export PATH=$ORACLE_HOME/bin:/usr/sbin:$PATH
```

#### 安装错误问题

```bash
vim $ORACLE_HOME/sysman/lib/ins_emagent.mk
```

💡 搜索$(MK_EMAGENT_NMECTL)替换为$(MK_EMAGENT_NMECTL) -lnnz11

#### 配置远程访问

```bash
SQL> alter user username account unlock
```