# Linux系统时间同步


说明：由于大数据中，集群对时间要求很高，所以集群内主机要经常同步时间（包括时区的同步）。


常用的手动进行时间的同步
	
	date -s "2017-03-03 03:03:03"

或者网络同步：
	
	yum install ntpdate
	ntpdate cn.pool.ntp.org

	
还可以进行如下的设置：

	1、yum install ntp

	2、vi /etc/ntp.conf

修改如下部分：

	server 58.220.207.226 iburst
	server 47.92.108.218 iburst
	server 202.112.29.82 iburst
	server 202.108.6.95 iburst
	
	其中ip为全球可用的ntp时间服务器，免费提供授时服务。
	
3、配置之后，保存设置，重启服务

	service ntpd stop

    service ntpd start
	
	
4、甚至可以设置crontab来定时与互联网服务器进行同步

<!--
create time: 2018-02-08 14:54:11
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

