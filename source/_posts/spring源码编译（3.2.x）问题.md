---
title: spring源码编译（3.2.x）问题
date: 2023-02-10 15:23:53
tags: [spring, 源码]
---

###### 替换build.gradle仓库地址

```
repositories {
        // maven { url "https://repo.spring.io/plugins-release" }
        maven { url "https://maven.aliyun.com/repository/spring-plugin" }
}
```

```
repositories {
        // maven { url "https://repo.spring.io/libs-release" }
        maven { url "http://maven.aliyun.com/nexus/content/groups/public" }
}
```
