# Linux 基本命令不能用的解决方法

###问题描述

最近某次，新建一个ssh客户端后，发现好多命令都不能用了比如：ls, vi, cat等，提示：

	-bash: XX: No such file or directory

但在其它还未关闭的ssh终端中可以使用，推测是/etc/profile文件的问题，显示$PATH后发现不对；切换root权限准备修改profile文件后，发现vi命令不能用，最后百度到了解决方法。

###解决方法

在ssh终端中执行下面语句，可以让此会话终端中环境变量起作用

```
export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/
usr/bin:/root/bin

```
然后修改/etc/profile文件，重新source后系统恢复正常

###后记

  后来找到原因是有人修改profile文件时，使用了$PATH=<newpath>，后面没有使用”:”拼接原来的$PATH，导致PATH丢失了重要环境变量
 
 [原文链接](http://blog.csdn.net/houmou/article/details/51020709)





<!--
create time: 2018-03-17 20:24:02
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

