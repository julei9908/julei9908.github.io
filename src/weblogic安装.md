# weblogic安装

#### 安装x11

```bash
yum -y install xorg-x11-xauth xorg-x11-utils
```

#### 安装jdk

```bash
vim /etc/profile

export JAVA_HOME=/usr/local/java/jdk1.8.0_191
export PATH=$PATH:$JAVA_HOME/bin

source /etc/profile
```

#### 创建用户

```bash
groupadd dba
useradd -g dba weblogic
passwd weblogic
```

#### 创建安装目录

```bash
mkdir -p /u02/
chown -R weblogic:dba /u02/
```

#### 安装

```bash
java -jar fmw_12.2.1.3.0_wls.jar
```