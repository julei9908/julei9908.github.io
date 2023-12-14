# mysql8.x（windows）

#### [安装visual c++](https://support.microsoft.com/zh-cn/topic/%E6%9C%80%E6%96%B0%E6%94%AF%E6%8C%81%E7%9A%84-visual-c-%E4%B8%8B%E8%BD%BD-2647da03-1eea-4433-9aff-95f26a218cc0)

#### 安装目录创建my.ini文件

```bash
[mysqld]
basedir=D:\\mysql
datadir=D:\\mydata\\data
```

#### 初始化

```bash
bin\mysqld --initialize --console
```

#### 服务安装（管理员权限）

```bash
bin\mysqld --install
```

#### 服务移除（管理员权限）

```bash
bin\mysqld --remove
```