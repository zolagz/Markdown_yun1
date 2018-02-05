# 4-1awk内容介绍

可以用来，统计，制表

学习内容

awk行处理方式和格式

awk内嵌参数应用

swk内嵌程序应用


4-2 awk处理方式

awk 一次处理一行内容

awk对每行可以切片处理

$ awk ‘{print $1}’   // 输出首个单词


使用awk 的格式

命令行格式(重点介绍)

$ awk [option] 'commadn' file(s)

* comman：有两部分组成；pattern {awk操作命令}

	* pattern ：正则表达式；逻辑判断式

脚本格式

$ awk -f awk-script-file file(s)



使用awk---基本格式

command1:pattern {awk操作命令}

操作命令：
	
* 内置函数：
	* print()   printf()
* 控制指令：
	* if(){...}else{...};
	* while(){...};


###awk内置参数应用

* awk内置变量1
	* $0 :表示整个当前行
	* $1 :每行第一个子段
	* $2 :每行第二个字段

* awk内置参数：分割符
	* option : -F field-separator(默认为空格)
	
	例如：$ awk -F ':' '{print$5}' /etc/passwd
	
	通过-F指定：为分隔符	 


```
[root@tom ~]# awk -F ':' '{print$5}' passwd 
root
bin
daemon
adm
lp
sync
shutdown
halt
mail

```

打印多个参数，通过逗号【,】分割

 awk -F ':' '{print $1,$5}' passwd 
 
```
[root@tom ~]#  awk -F ':' '{print $1,$5}' passwd 
root root
daemon daemon
adm adm
avahi-autoipd Avahi IPv4LL Stack
systemd-bus-proxy systemd Bus Proxy
systemd-network systemd Network Management
dbus System message bus
polkitd User for polkitd
tss Account used by the trousers package to sandbox the tcsd sshd Privilege-separated SSH
alfred alfred
apache Apache
[root@tom ~]# 

```

通过加入字段分割；比如加入 "  "

awk -F ':' '{print $1"  "$5}' passwd 

或者加入\t

awk -F ':' '{print $1 " \t " $5}' passwd

```
# awk -F ':' '{print $1"  "$5}' passwd 
root  root
bin  bin
daemon  daemon
adm  adm
lp  lp
sync  sync
shutdown  shutdown
halt  halt
mail  mail
operator  operator
games  games
ftp  FTP User
nobody  Nobody
avahi-autoipd  Avahi IPv4LL Stack
systemd-bus-proxy  systemd Bus Proxy
systemd-network  systemd Network Management
dbus  System message bus
polkitd  User for polkitd
tss  Account used by the trousers package to sandbox the tcsd daemon
```
awk -F ':' '{print $1 " \t " $5}' passwd


```

[root@tom ~]# awk -F ':' '{print $1" \t "$5}' passwd 
root     root
bin      bin
daemon   daemon
adm      adm
lp       lp

```

awk -F ':' '{print "User:"$1 " \t " "Message:"$5}' passwd

```
[root@tom ~]# awk -F ':' '{print "User:"$1 " \t " "Message:"$5}' passwd
User:root        Message:root
User:bin         Message:bin
User:daemon      Message:daemon
User:adm         Message:adm
User:lp          Message:lp
User:sync        Message:sync
User:shutdown    Message:shutdown
User:halt        Message:halt
User:mail        Message:mail
User:operator    Message:operator
User:games       Message:games
User:ftp         Message:FTP User
```


### awk内置参数应用

* awk内置变量
	* NR ：每行的记录号 (行)
	* NF ：字段数量变量  （列）
	* FILENAME : 正在处理的文件名


awk -F ':' '{print NR NF}' passwd

输入文件名

awk -f ':' '{print FILENAME}' passwd



<!--
create time: 2018-02-05 11:02:41
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

