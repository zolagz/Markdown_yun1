# mvn使用方式



1. clean

```
	#清除包
	mvn clean
```
2.install

```
#全部安装（包含打包）
mvn install 
```

3.package

```
#打包(但是不会将最新的代码加载到mvn库，使用install更好)
mvn package
```
3.install -Dmaven.test.skip=true

```
#跳过测试项安装
mvn install -Dmaven.test.skip=true
```
4.clean source:jar deploy

```
#deploy在jenkins中可以通过pom.xml将私服中的（代码+依赖包）拉取下来编译到目录
#使用这项，一般是避免jar包冲突和上线的时候节约编译时间
#使用方法：
jenkins中：
    1.创建mvn项目
    2.按照步骤往下填
    3.有一行有pom.xml，在该行的上方填入clean source:jar deploy
    4.以后做发布的时候，可以调用这些编译好的jar包或者war包

```

<!--
create time: 2018-07-04 11:52:02
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

