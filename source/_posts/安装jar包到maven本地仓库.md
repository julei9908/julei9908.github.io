---
title: 安装jar包到maven本地仓库
date: 2021-08-22 14:45:59
tags: maven
---

```shell
mvn install:install-file -Dfile=文件所在路径 -DgroupId=包名 -DartifactId=项目名 -Dversion=版本号 -Dpackaging=jar -DgeneratePom=true -DcreateChecksum=true
```
