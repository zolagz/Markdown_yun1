# Hive环境搭建

内置 derby 版:
解压 hive 安装包bin/hive 启动即可使用缺点:不同路径启动 hive，每一个 hive 拥有一套自己的元数据，无法共享


###mysql版本的hive安装

Hive安装前需要安装装JDK和Hadoop,配置好环境变量

1.上传hive.tar包

2.解压  tar -zxvf hive-1.2.1.tar.gz 


3.安装mysql

设置mysql运行远程登录


查看myslq的运行状态

```
[root@node1 ~]# service mysqld status
mysqld (pid  1577) is running...
```

设置mysql开机启动

```
[root@node1 ~]# chkconfig mysqld on
[root@node1 ~]# 
```

查看状态

```
[root@node1 ~]# chkconfig mysqld --list
mysqld          0:off   1:off   2:on    3:on    4:on    5:on    6:off
[root@node1 ~]# 
```
4. 配置hive

 a）配置HIVE_HOME环境变量 

进入解压后的hive文件目录，修改conf/hive-env.sh.template

cp hive-env.sh.template hive-env.sh

vi hive-env.sh

![](http://p2ehgqigv.bkt.clouddn.com/18-2-26/67936958.jpg)

克隆一个会话，通过which hadoop得到hadoop的所在目录
![](http://p2ehgqigv.bkt.clouddn.com/18-2-26/80228758.jpg)

保存退出预览
![](http://p2ehgqigv.bkt.clouddn.com/18-2-26/57120596.jpg)


b）配置元数据库信息  
		vi  hive-site.xml 
		添加如下内容：

```
<configuration>

<!--指定连接的msyql数据库-->
		<property>
		<name>javax.jdo.option.ConnectionURL</name>
		<value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true</value>
		<description>JDBC connect string for a JDBC metastore</description>
		</property>

<!--连接数据库的驱动-->
		<property>
		<name>javax.jdo.option.ConnectionDriverName</name>
		<value>com.mysql.jdbc.Driver</value>
		<description>Driver class name for a JDBC metastore</description>
		</property>

<!--配置连接数据库的用户名-->
		<property>
		<name>javax.jdo.option.ConnectionUserName</name>
		<value>root</value>
		<description>username to use against metastore database</description>
		</property>

<!--配置连接数据库的密码-->
		<property>
		<name>javax.jdo.option.ConnectionPassword</name>
		<value>admin</value>
		<description>password to use against metastore database</description>
		</property>
</configuration>
```
5. 上传连接mysql的数据库驱动到hive的lib目录下

[mysql-connector-java-5.1.32](https://pan.baidu.com/s/1bqkqDMR)


6.启动hive 
![](http://p2ehgqigv.bkt.clouddn.com/18-2-26/37739856.jpg)

<!--
create time: 2018-02-26 14:48:39
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

