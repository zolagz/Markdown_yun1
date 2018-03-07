# Sqoop的安装


##sqoop 介绍

Sqoop 是 Hadoop 和关系数据库服务器之间传送数据的一种工具。它是用来从关系数据 库如:MySQL，Oracle 到 Hadoop 的 HDFS，并从 Hadoop 的文件系统导出数据到关系数据 库。由 Apache 软件基金会提供。

###sqoop 安装：

安装 sqoop 的前提是已经具备 java 和 hadoop 的环境。 

最新稳定版: 1.4.6配置文件修改:

```cd $SQOOP_HOME/confmv sqoop-env-template.sh sqoop-env.shvi sqoop-env.shexport HADOOP_COMMON_HOME=/root/apps/hadoop/ 

export HADOOP_MAPRED_HOME=/root/apps/hadoop/

export HIVE_HOME=/root/apps/hive

```
###加入 mysql 的 jdbc 驱动包
	cp /hive/lib/mysql-connector-java-5.1.28.jar $SQOOP_HOME/lib/ 

###验证启动

本命令会列出所有 mysql 的数据库。

```bin/sqoop list-databases --connect jdbc:mysql://localhost:3306/ -- username root --password hadoop;
```

到这里，整个 Sqoop 安装工作完成。



<!--
create time: 2018-03-06 20:45:55
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

