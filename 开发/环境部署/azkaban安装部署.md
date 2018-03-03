# azkaban安装部署



Azkaban 调度器 

3.1. Azkaban 介绍
Azkaban 是由 Linkedin 公司推出的一个批量工作流任务调度器，用于在一个工作流内Azkaban 功能特点:  以一个特定的顺序运行一组工作和流程。Azkaban 使用 job 配置文件建立任务之间的依赖关系，并提供一个易于使用的 web 用户界面维护和跟踪你的工作流。  提供功能清晰，简单易用的 Web UI 界面
 提供 job 配置文件快速建立任务和任务之间的依赖关系 提供模块化和可插拔的插件机制，原生支持 command、Java、Hive、Pig、Hadoop 基于 Java 开发，代码结构清晰，易于二次开发3.2. Azkaban 安装部署 Azkaban 的组成如下:
mysql 服务器:用于存储项目、日志或者执行计划之类的信息
   web 服务器:使用 Jetty 对外提供 web 服务，使用户可以通过 web 页面方便管理 

executor 服务器:负责具体的工作流的提交、执行。Azkaban 有两种部署方式: 和   。   solo server modecluster server mode 
  solo server mode(单机模式):该模式中 webServer 和 executorServer 运行在同一个进 程中，进程名是 AzkabanSingleServer。可以使用自带的 H2 数据库或者配置 mysql 数据。   该模式适用于小规模的使用。 cluster server mode(集群模式):该模式使用 MySQL 数据库，webServer 和 executorServer 运行在不同进程中，该模式适用于大规模应用。 其实在单机模式中，AzkabanSingleServer 进程只是把 AzkabanWebServer 和 AzkabanExecutorServer 合到一起启动而已。 
准备工作Azkaban Web 服务器
azkaban-web-server-2.5.0.tar.gzAzkaban 执行服务器azkaban-executor-server-2.5.0.tar.gzMySQL本文档中默认已安装好 mysql 服务器。
下载地址:http://azkaban.github.io/downloads.html上传安装包将安装包上传到集群,最好上传到安装 hive、sqoop 的机器上,方便命令的执行。新建 azkaban 目录,用于存放 azkaban 运行程序。azkaban web 服务器安装解压 azkaban-web-server-2.5.0.tar.gz命令: tar –zxvf azkaban-web-server-2.5.0.tar.gz将解压后的 azkaban-web-server-2.5.0 移动到 azkaban 目录中,并重新命名 webserver

命令: mv azkaban-web-server-2.5.0 ../azkaban                   cd ../azkabanmv azkaban-web-server-2.5.0 serverazkaban 执行服器安装解压 azkaban-executor-server-2.5.0.tar.gz命令:tar –zxvf azkaban-executor-server-2.5.0.tar.gz将解压后的 azkaban-executor-server-2.5.0 移动到 azkaban 目录中,并重新命 名 executor命令:mv azkaban-executor-server-2.5.0 ../azkaban cd ../azkabanmv azkaban-executor-server-2.5.0 executorazkaban 脚本导入解压: azkaban-sql-script-2.5.0.tar.gz命令:tar –zxvf azkaban-sql-script-2.5.0.tar.gz 将解压后的 mysql 脚本,导入到 mysql 中:进入 mysql```mysql> create database azkaban;mysql> use azkaban;Database changedmysql> source /home/hadoop/azkaban-2.5.0/create-all-sql-2.5.0.sql;

```###创建 SSL 配置(https)命令: keytool -keystore keystore -alias jetty -genkey -keyalg RSA运行此命令后,会提示输入当前生成 keystor 的密码及相应信息,输入的密码请劳记, 信息如下:

```输入 keystore 密码: 再次输入新密码: 您的名字与姓氏是什么?
\[Unknown]: 您的组织单位名称是什么?
\[Unknown]: 您的组织名称是什么?\[Unknown]: 您所在的城市或区域名称是什么?\[Unknown]: 您所在的州或省份名称是什么?\[Unknown]: 该单位的两字母国家代码是什么\[Unknown]: CNCN=Unknown, OU=Unknown, O=Unknown, L=Unknown, ST=Unknown, C=CN 正确吗?[否]: y 输入<jetty>的主密码(如果和 keystore 密码相同，按回车):再次输入新密码:
```完成上述工作后,将在当前目录生成 keystore 证书文件,将 keystore 拷贝到 azkaban web 服务器根目录中.如:cp keystore azkaban/webserver###配置文件注:先配置好服务器节点上的时区先生成时区配置文件 Asia/Shanghai，用交互式命令 tzselect 即可 拷贝该时区文件，覆盖系统本地时区配置	cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime###azkaban web 服务器配置进入 azkaban web 服务器安装目录 conf 目录 修改 azkaban.properties 文件

```#Azkaban Personalization Settings azkaban.name=Test azkaban.label=My Local Azkaban azkaban.color=#FF3601 azkaban.default.servlet.path=/index web.resource.dir=web/ default.timezone.id=Asia/Shanghai#服务器 UI 名称,用于服务器上方显示的名字 #描述#UI 颜色 ##默认根 web 目录 #默认时区,已改为亚洲/上海 默认为美国#Azkaban UserManager class user.manager.class=azkaban.user.XmlUserManager #用户权限管理默认类user.manager.xml.file=conf/azkaban-users.xml#Loader for projects executor.global.properties=conf/global.properties azkaban.project.dir=projectsdatabase.type=mysql mysql.port=3306 mysql.host=hadoop03 mysql.database=azkaban mysql.user=root mysql.password=root mysql.numconnections=100# Velocity dev mode velocity.dev.mode=false# Jetty 服务器属性. jetty.maxThreads=25 jetty.ssl.port=8443 jetty.port=8081 jetty.keystore=keystore jetty.password=123456 jetty.keypassword=123456 jetty.truststore=keystore jetty.trustpassword=123456# 执行服务器属性 executor.port=12321# 邮件设置 mail.sender=xxxxxxxx@163.com mail.host=smtp.163.com mail.user=xxxxxxxx mail.password=********** job.failure.email=xxxxxxxx@163.com#用户配置,具体配置参加下文# global 配置文件所在位置 ##数据库类型 #端口号#数据库连接 IP #数据库实例名#数据库用户名 #数据库密码#最大连接数#最大线程数 #Jetty SSL 端口 #Jetty 端口#SSL 文件名 #SSL 文件密码#Jetty 主密码 与 keystore 文件相同 #SSL 文件名# SSL 文件密码#执行服务器端口#发送邮箱#发送邮箱 smtp 地址#发送邮件时显示的名称 #邮箱密码#任务失败时发送邮件的地址job.success.email=xxxxxxxx@163.com lockdown.create.projects=false cache.directory=cache#任务成功时发送邮件的地址 ##缓存目录


```

azkaban 执行服务器配置进入执行服务器安装目录 conf,修改 azkaban.properties

```vi azkaban.properties#Azkaban default.timezone.id=Asia/Shanghai# Azkaban JobTypes 插件配置 azkaban.jobtype.plugin.dir=plugins/jobtypes#Loader for projects executor.global.properties=conf/global.properties azkaban.project.dir=projects#数据库设置 database.type=mysql mysql.port=3306 mysql.host=192.168.20.200 mysql.database=azkaban mysql.user=azkaban mysql.password=oracle mysql.numconnections=100# 执行服务器配置 executor.maxThreads=50 executor.port=12321 executor.flow.threads=30#时区#jobtype 插件所在位置#数据库类型(目前只支持 mysql) #数据库端口号#数据库 IP 地址 #数据库实例名 #数据库用户名#数据库密码 #最大连接数#最大线程数 #端口号(如修改,请与 web 服务中一致)#线程数

```### 用户配置进入 azkaban web 服务器 conf 目录,修改 azkaban-users.xml vi azkaban-users.xml 增加 管理员用户

```
<azkaban-users><user username="azkaban" password="azkaban" roles="admin" groups="azkaban" /> <user username="metrics" password="metrics" roles="metrics"/><user username="admin" password="admin" roles="admin,metrics" /><role name="admin" permissions="ADMIN" /><role name="metrics" permissions="METRICS"/> 
</azkaban-users>
  
```
###  启动web 服务器在 azkaban web 服务器目录下执行启动命令
	bin/azkaban-web-start.sh注:在 web 服务器根目录运行###执行服务器在执行服务器目录下执行启动命令	bin/azkaban-executor-start.sh ./注:只能要执行服务器根目录运行启动完成后,在浏览器(建议使用谷歌浏览器)中输入 https://服务器 IP 地址:8443 ,即可访问 azkaban 服务了.在登录中输入刚才新的户用名及密码,点击 login.

<!--
create time: 2018-02-27 22:01:04
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

