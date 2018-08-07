# 安装tomcat


###  离线安装tomcat

上传apache-tomcat-7.0.47.tar.gz至虚拟机

解压

```
[root@localhost home]# tar -zxvf apache-tomcat-7.0.47.tar.gz
```

启动Tomcat服务

```
[root@localhost home]# ./apache-tomcat-7.0.47/bin/startup.sh  
```

关闭Tomcat服务 

```
[root@localhost home]# ./apache-tomcat-7.0.47/bin/shutdown.sh   --

```

http://{IP}:8080/ 若无法访问，则可能是防火墙没有关闭（详见 下方讲解）

设置开机启动tomcat（可选）

```
[root@localhost ~]# vi /etc/rc.d/rc.local 
```
在末尾添加/home/apache-tomcat-7.0.47/bin/startup.sh

###关闭防火墙

####CentOS 6.X      
如果无法通过http协议访问虚拟，需要关闭防火墙，防火墙相关命令；

暂停下次开机会自动开启防火墙

service iptables stop 

永久关闭下次开机生效

chkconfig iptables off 
 
检查状态

service iptables status 

iptables: Firewall is not running. 运行关闭后结果 关闭步骤如下

![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/81996021.jpg)
￼
#### CentOS 7.X 

防火墙配置跟以前版本有很大区别，经过大量尝试，终于找到解决问题的关键
CentOS7这个版本的防火墙默认使用的是firewall，与之前的版本使用iptables不一样。按如下配置防火墙：

```
关闭防火墙：sudo systemctl stop firewalld.service
```
![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/92473993.jpg)

```
￼关闭开机启动：sudo systemctl disable firewalld.service
```
![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/16512611.jpg)
￼

3、安装iptables防火墙
执行以下命令安装iptables防火墙：sudo yum install iptables-services
4、配置iptables防火墙，打开指定端口（具体跟以前版本一样，网上介绍很多，这里不多介绍了）
 5. 设置iptables防火墙开机启动：sudo systemctl enable iptables
OK了，根据配置的端口就可以访问了。

 
####允许端口访问配置
在实际的开发工作中，我们一般是不建议永久关闭防火墙的。但是我们现在是学习阶段只是为了方便。大家一定要注意这个问题。所以我们要知道怎么将端口放行的。具体设置如下：
以允许数据库端口为例（3306）编辑 文件
（以下命令操作均为root用户）

```
1、编辑iptables文件
vi /etc/sysconfig/iptables
\# 添加如下一行，可以参照已有的已经开启的ssh的22端口
-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
\#（添加3306为其他端口，即可开启其他端口，如80，21等）
2、重启防火墙
service iptables restart

```



<!--
create time: 2018-01-14 15:09:50
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

