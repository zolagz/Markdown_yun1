# Ubunto su 密码

[原文链接](https://blog.csdn.net/david_xtd/article/details/7229325)

Ubuntu刚安装后，不能在terminal中运行su命令，因为root没有默认密码，需要手动设定。


以安装ubuntu时输入的用户名登陆，该用户在admin组中，有权限给root设定密码。
![](http://p2ehgqigv.bkt.clouddn.com/18-6-17/274565.jpg)

给root用户设置密码的具体步骤：

1. 打开一个terminal，然后输入下面的命令

sudo passwd [root]


 回车后会出现让你输入原始密码，新密码和确认密码，

 [sudo] password for you ：---> 输入你的密码（你现在这个用户的密码），不回
 显
 
 Enter new UNIX password: --- > 设置root 密码

Retype new UNIX password: --> 重复这样

这样你的root的密码设置好了。

注：root可以省略，命令为passwd而不是password，我犯过这个错误。

2. 在terminal中利用su命令就可以切换到root用户了。

注：su和sudo的区别是：1). su的密码是root的密码，而sudo的密码是用户的密码；

2). su直接将身份变成root，而sudo是以用户登录后以root的身份运行命令，不需要知道root密码；



<!--
create time: 2018-06-17 10:05:51
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

