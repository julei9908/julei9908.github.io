# maven本地仓库安装jar包

```bash
mvn install:install-file -Dfile=文件所在路径 -DgroupId=包名 -DartifactId=项目名 -Dversion=版本号 -Dpackaging=jar -DgeneratePom=true -DcreateChecksum=true
```