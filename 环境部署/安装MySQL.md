# 安装MySQL

### mysql 忘记密码

[参考链接](https://www.cnblogs.com/gumuzi/p/5711495.html)

vi /etc/my.cnf

在[mysqld]后面任意一行添加“skip-grant-tables”用来跳过密码验证的过程
![](https://images2015.cnblogs.com/blog/862200/201607/862200-20160727153846934-2016310105.png)
skip-grant-tables 

保存文档并退出：

2.接下来我们需要重启MySQL：
systemctl restart mysql.service

3重启之后输入#mysql即可进入mysql。

![](https://images2015.cnblogs.com/blog/862200/201607/862200-20160727154613466-19775596.png)

接下来就是用sql来修改root的密码

```
mysql> use mysql;
mysql> update user set password=password("你的新密码") where user="root";
mysql> flush privileges;
mysql> quit

```
到这里root账户就已经重置成新的密码了。

5.编辑my.cnf,去掉刚才添加的内容，然后重启MySQL。大功告成！




----------------------👇 2018-07-09更新------------------------

在centos 7 上，安装mysql后，安装的是MariaDB版本

###安装后

成功安装MariaDB后，设置root密码。 全新安装将具有空白密码。 输入以下内容设置新密码 -

>mysqladmin -u root password "[enter your password here]"

例如

```
# mysqladmin -u root password "admin"
# mysql -uroot -padmin
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 10.0.35-MariaDB-wsrep MariaDB Server, wsrep_25.23.rc3fc46e

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```
----------------------👇 2018-02-26更新------------------------
###yum 安装mysql

yum install mysql mysql-server mysql-devel 


完成后，用  /etc/init.d/mysqld start    启动mysql

启动mysql控制台： 

```
mysql>USE mysql; 

mysql>UPDATE user SET Password=PASSWORD('newpassword') WHERE 
user='root'; 

mysql>FLUSH PRIVILEGES; 
     允许远程登录 

mysql -u root -p 
Enter Password: <your new password> 

mysql>GRANT ALL PRIVILEGES ON *.* TO '用户名'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION; 

mysql>FLUSH PRIVILEGES; 
    完成后就能远程管理mysql了。

mysql服务名字   service  mysqld start
   
   
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION; 

 UPDATE user SET Password=PASSWORD('hadoop') WHERE user='root'; 

```
----------------------👆2018-02-26更新------------------------

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
 
第六步：远程连接授权。

	GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'admin' WITH GRANT OPTION;

###注意：'root'、**'admin'** 需要替换成实际的用户名和密码。


###执行完后切记执行以下命令：

```
mysql>FLUSH PRIVILEGES;

```


###查看修改后的用户信息

	mysql>select host,user,password from user;
	
	
如果密码设置错了，用下面的方法修改；
###修改密码；

 	mysql> update user set password = 'admin' where user = '%';


[参考链接](https://www.jianshu.com/p/2614c15d7c4d)

<!--
create time: 2018-01-14 15:17:46
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

