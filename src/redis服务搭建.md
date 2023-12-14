# redis服务搭建

#### 安装依赖包

```bash
yum -y install tcl
```

#### 解压源码包

```bash
tar -zxf redis-4.0.9.tar.gz
```

#### 编译

```bash
make
make test
```

#### 安装

```bash
make PREFIX=/usr/local/redis install
```

#### 复制配置文件

```bash
cp redis.conf  /etc/redis/6379.conf
```

#### 设置自启动

1. 编辑6379.conf设置daemonize为yes

2. 复制自启动脚本

```bash
cp utils/redis_init_script  /etc/init.d/redisd
```

3. 修改启动脚本参数

💡 #!/bin/sh的下方添加：chkconfig: 2345 90 10

```bash
REDISPORT=6379
EXEC=/usr/local/redis/bin/redis-server
CLIEXEC=/usr/local/redis/bin/redis-cl
```

4. 设置自启动

```bash
chkconfig redisd on
```

5. 启动

```bash
systemctl start redisd
```