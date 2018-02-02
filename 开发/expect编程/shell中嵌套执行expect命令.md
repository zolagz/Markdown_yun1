# shell中嵌套执行expect命令


shell中嵌套执行expect命令实例

这篇文章主要介绍了shell中嵌套执行expect命令实例,一直都想把expect的操作写到bash脚本里,这样就不用我再写两个脚本来执行了,需要的朋友可以参考下

一直都想把expect的操作写到bash脚本里,这样就不用我再写两个脚本来执行了,搞了一下午终于有点小成就,给大家看看吧.

系统:centos 5.x

1.先安装expect

复制代码 代码如下:

	yum -y install expect

2.脚本内容:

复制代码 代码如下:

```
cat auto_svn.sh
#!/bin/bash
passwd='123456'
/usr/bin/expect <<-EOF
set time 30
spawn ssh -p18330 root@192.168.10.22
expect {
"*yes/no" { send "yes\r"; exp_continue }
"*password:" { send "$passwd\r" }
}
expect "*#"
send "cd /home/trunk\r"
expect "*#"
send "svn up\r"
expect "*#"
send "exit\r"
interact
expect eof
EOF
```

这样写的话,就方便得很多,一个脚本就包括完了.

<!--
create time: 2018-02-02 17:15:14
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

