# Linux免密登录


###Linux下生成秘钥

执行如下命令：

	ssh-keygen -t rsa
![](http://p2ehgqigv.bkt.clouddn.com/18-2-1/46328938.jpg)
	
执行之后会在用户的根目录生成一个 “.ssh”的文件夹

![](http://p2ehgqigv.bkt.clouddn.com/18-2-1/33779506.jpg)

进入“.ssh”会生成以下几个文件
![](http://p2ehgqigv.bkt.clouddn.com/18-2-1/52856713.jpg)

###远程免密登录

####通过ssh-copy-id上传远程服务器

命令： ssh-copy-id -i ~/.ssh/id_rsa.put <romte_ip>

举例：

```
[root@test .ssh]# ssh-copy-id -i ~/.ssh/id_rsa.pub 192.168.91.135 
root@192.168.91.135's password: 
Now try logging into the machine, with "ssh '192.168.91.135'", and check in:
 
.ssh/authorized_keys
 
to make sure we haven't added extra keys that you weren't expecting.
 
[root@node3 .ssh]# ssh root@192.168.91.135
Last login: Mon Oct 10 01:25:49 2016 from 192.168.91.133
[root@localhost ~]#

```

![](http://p2ehgqigv.bkt.clouddn.com/18-2-1/616778.jpg)

通过ssh <ip>登录测试

常见错误：

```　　　
[root@node1 ~]# ssh-copy-id -i ~/.ssh/id_rsa.pub 192.168.91.135

-bash: ssh-copy-id: command not found //提示命令不存在

解决办法：yum -y install openssh-clients
```


参考链接：[Linux下实现免密码登录(超详细)](http://www.jb51.net/article/94599.htm)


<!--
create time: 2018-02-01 19:31:37
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

