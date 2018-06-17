# Ubunto 16.04开启ssh服务

[原文链接](https://www.cnblogs.com/EasonJim/p/7189509.html)

安装：

```
sudo apt-get install openssh-server
```
启动：

```
sudo service ssh start
```
查询服务启动状态：

```
sudo ps -e | grep ssh
或者
sudo service ssh status
```
配置开机启动：

```
sudo sysv-rc-conf
```
![](http://p2ehgqigv.bkt.clouddn.com/18-6-17/17649889.jpg)

把四项都选上。

修改默认端口：

```
sudo vim /etc/ssh/sshd_config
```

找到Port 22，然后改成相应的端口。建议先保留22端口的。后面连接成功了之后再删除22端口，保险起见。


![](http://p2ehgqigv.bkt.clouddn.com/18-6-17/41352844.jpg)

重启服务

```
sudo service ssh restart
```
查看状态

```
netstat -an | grep "LISTEN "
```


![](http://p2ehgqigv.bkt.clouddn.com/18-6-17/81817687.jpg)

用端口进行连接

```
ssh username@ip -p 50022
```
连接成功之后记得把22端口去除。 

注意：如果开通了ufw防火墙的要加入规则，默认端口为22。在Ubuntu下防火墙建议使用ufw进行管理，主要是方便操作。


<!--
create time: 2018-06-17 10:11:30
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

