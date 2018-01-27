# 安装MySQL



第一步：查看mysql是否安装。

	rpm -qa|grep mysql
 
第二步：如果mysql的版本不是想要的版本。需要把mysql卸载。

	yum remove mysql mysql-server mysql-libs mysql-common

	rm -rf /var/lib/mysql

	rm /etc/my.cnf
 
第三步：安装mysql。需要使用yum命令安装。在安装mysql之前需要安装mysql的下载源。需要从oracle的官方网站下载。

1）下载mysql的源包。[网盘下载](https://pan.baidu.com/s/1kXb13Ht)

<del> 我们是centos6.4 </del> 对应的rpm包为：mysql-community-release-el6-5.noarch.rpm

2）安装mysql下载源：

	yum localinstall mysql-community-release-el6-5.noarch.rpm 

3）在线安装mysql：

	yum install mysql-community-server
 
第四步：启动mysql

	service mysqld start
 
第五步：需要给root用户设置密码。

	/usr/bin/mysqladmin -u root password 'new-password'　　// 为root账号
设置密码
 
第六步：远程连接授权。参考这里[](https://www.jianshu.com/p/2614c15d7c4d)

	GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'admin' WITH GRANT OPTION;

###注意：'root'、**'admin'** 需要替换成实际的用户名和密码。

<!--
create time: 2018-01-14 15:17:46
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

