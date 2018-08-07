# httpd介绍

安装方法

yum install httpd

启动服务

service httpd start

资源目录在

/var/www/




只下载不安装；

第一步：查看yum安装插件
 rpm -qa | grep yum  
 
 ```
[root@node1 ~]# rpm -qa | grep yum

yum-plugin-fastestmirror-1.1.30-30.el6.noarch

yum-metadata-parser-1.1.2-16.el6.x86_64

yum-3.2.29-69.el6.centos.noarch
 ```
 
 如果有这几个就可以直接yum只下载到指定目录而不安装了，downloadonly：仅下载，downloaddir：指定下载目录：
 
 [root@node1 ~]# yum install wget --downloadonly --downloaddir=/home/my_download -y
 
 
 注：如果不支持那个参数，则可以
 
 yum upgrade yum -y  
 
 wget进行下载
 
 若无安装，请看备注附件，-P：指定下载目录：
 
 [root@node1 ~]# wget http://192.168.25.120/soft/jdk-7u75-linux-x64.tar.gz -P /usr/local/

yum指定路径安装：

yum -y install subversion --installroot=/home/svninstall/ 

或者rpm安装

 rpm -ivh wget-1.12-10.el6.x86_64.rpm 
 
 yum查看是否安装软件：
 
 [root@node1 ~]# yum list installed subversion*    

yum查看yum服务器上可下载软件版本信息：

[root@node1 ~]# yum list | grep subversion  


    列出所有可安裝的软件清单:yum list  
    列出所有可更新的软件清单：yum check-update  
    安装所有更新软件：yum update  
    仅安装指定的软件：yum install <package_name>  
    仅更新指定的软件：yum update <package_name>  
    用YUM删除软件包：yum remove <package_name>  
    
[上述参考链接](http://blog.csdn.net/typa01_kk/article/details/49227663)

rpm包安装方法

rpm -ivh 安装包

1、找到相应的软件包，比如soft.version.rpm，下载到本机某个目录；
2、打开一个终端，su -成root用户；
3、cd soft.version.rpm所在的目录；
4、输入rpm -ivh soft.version.rpm


二、deb包安装方式步骤：

1、找到相应的软件包，比如soft.version.deb，下载到本机某个目录；
2、打开一个终端，su -成root用户；
3、cd soft.version.deb所在的目录；
4、输入dpkg -i soft.version.deb


<!--
create time: 2018-02-03 10:07:47
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

