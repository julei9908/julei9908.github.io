# rabbitmq安装

#### 安装erlang

```bash
vim /etc/yum.repos.d/rabbitmq-erlang.repo

# In /etc/yum.repos.d/rabbitmq-erlang.repo
[rabbitmq-erlang]
name=rabbitmq-erlang
baseurl=https://dl.bintray.com/rabbitmq-erlang/rpm/erlang/21/el/7
gpgcheck=1
gpgkey=https://dl.bintray.com/rabbitmq/Keys/rabbitmq-release-signing-key.asc
repo_gpgcheck=0
enabled=1

yum install erlang
```

#### 安装rabbitmq

```bash
vim /etc/yum.repos.d/rabbitmq.repo

[bintray-rabbitmq-server]
name=bintray-rabbitmq-rpm
baseurl=https://dl.bintray.com/rabbitmq/rpm/rabbitmq-server/v3.7.x/el/7/
gpgcheck=0
repo_gpgcheck=0
enabled=1

yum install rabbitmq-server
```

#### 启用web插件

```bash
rabbitmq-plugins enable rabbitmq_management
```

#### 启动rabbitmq

```bash
systemctl start rabbitmq-server
```

#### 创建用户

```bash
rabbitmqctl add_user julei 123456
rabbitmqctl set_user_tags julei administrator
rabbitmqctl set_permissions -p / julei ".*" ".*" ".*"
```