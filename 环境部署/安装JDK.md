# 安装JDK

CentOS卸载系统自带的OpenJDK并安装Sun的JDK的方法

查看目前系统的jdk:
rpm -qa | grep jdk

```
[root@localhost ~]# rpm -qa|grep jdk
java-1.6.0-openjdk-1.6.0.35-1.13.7.1.el6_6.i686
java-1.7.0-openjdk-1.7.0.79-2.5.5.4.el6.i686
```


卸载之：（保证卸载的jdk版本是自己的系统显示的）

```
[root@localhost ~]# yum -y remove java-1.6.0-openjdk-1.6.0.35-1.13.7.1.el6_6.i686
[root@localhost ~]# yum -y remove java-1.7.0-openjdk-1.7.0.79-2.5.5.4.el6.i686

```
将 jdk-7u75-linux-x64.tar [网盘下载](https://pan.baidu.com/s/1bq25XHP) 上传至虚拟机

```
[root@localhost jdk]# tar zxvf jdk-7u75-linux-x64.tar 
```
 
###将jdk1.7.0_55移动到 /opt/目录下

```
[root@localhost jdk]# mv jdk1.7.0_75/ /opt/

```
编辑系统配置文件/etc/profile增加环境变量

```
[root@localhost jdk]# vi /etc/profile  

```
在文件末尾追加以下代码：

```
export JAVA_HOME=/opt/jdk1.7.0_75
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```
保存并退出
设置当前配置立即生效

```
[root@localhost jdk]# source /etc/profile
```
显示以下结果表示安装正确
![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/94442076.jpg)
￼

<!--
create time: 2018-01-14 15:04:07
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

