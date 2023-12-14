# tomcat安装

#### 解压缩

```bash
tar -zxf apache-tomcat-7.0.85.tar.gz
```

#### 重命名

```bash
mv apache-tomcat-7.0.85 tomcat
```

#### 开机自启

```bash
vim /etc/rc.d/rc.local

export JAVA_HOME=/usr/local/java/jdk1.8.0_191
/usr/local/tomcat/bin/startup.sh start
```

#### 修改rc.local权限

```bash
chmod 755 /etc/rc.d/rc.local
```