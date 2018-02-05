# 4-5awk逻辑判断式

command :pattern { awk 操作命令 }

patter:正则表达式；逻辑判断式

逻辑表达式：

~ ,!~ :匹配正则表达式

== ,!=,< , >   :逻辑判断表达式

例子：

打印用：分割的第一个字段以a开头的内容

awk -F ':' '$1~/^a.*/{print $1}' passwd

```
[root@tom ~]# awk -F ':' '$1~/^a.*/{print $1}' passwd
adm
avahi-autoipd
alfred
apache
[root@tom ~]# 

```

对上面操作的取反

 awk -F ':' '$1!~/^a.*/{print $1}' passwd
 
 逻辑判断uid>100，以:分割的第一个字段和第三个字段
 
 awk -F ':' '$3>100{print $1,$3}' passwd
 
 ```
[root@tom ~]#  awk -F ':' '$3>100{print $1,$3}' passwd
avahi-autoipd 170
systemd-bus-proxy 999
systemd-network 998
polkitd 997
alfred 1000
[root@tom ~]# 

 ```
 
 awk -F ':' '$3<100{print $1,$3}' passwd
 
 
 awk -F ':' '$3==100{print $1,$3}' passwd
 
 awk -F ':' '$3!=100{print $1,$3}' passwd
 
 
 
4-7 awk 的扩展格式
 
 BEGIN{print "start"} pattern {commadns} END{ print "END"}
 
 
 
 案例1
 
 制表显示/etc/passwd每行的行号，每行的列数，对应行的用户名
 
 awk -F ':' 'BEGIN{print "Line    Col    User"}{print NR,NF,$1}END{print "------FILENAME-----"}' passwd
 
 ```
[root@tom ~]# awk -F ':' 'BEGIN{print "Line    Col    User"}{print NR,NF,$1}END{print "------FILENAME-----"}' passwd

Line    Col    User
1 7 root
2 7 bin
3 7 daemon
4 7 adm
5 7 lp
6 7 sync
7 7 shutdown
8 7 halt
9 7 mail
10 7 operator
11 7 games
12 7 ftp

 ```
 
 案例4 统计当前文件夹下的文件/文件夹占用的大小

首先查看下当前文件夹下的内容

把第五个字段相加
 
 ```
[root@tom ~]# ll
total 28
-rw-r--r--. 1 root root   37 Feb  4 00:09 123.txt
-rw-r--r--. 1 root root   37 Feb  4 00:16 abc.txt
-rw-------. 1 root root 1139 Jan 25 05:27 anaconda-ks.cfg
drwxr-xr-x. 2 root root 4096 Feb  3 08:01 demo1
drwxr-xr-x. 2 root root 4096 Feb  3 06:35 guan
-rw-r--r--. 1 root root 1142 Feb  3 19:04 passwd
drwxr-xr-x. 2 root root   59 Feb  3 23:14 shell
drwxr-xr-x. 2 root root 4096 Feb  3 06:09 sh_test
[root@tom ~]# 

 ```
 
 ls -l | awk 'BEGIN{size=0}{size+=$5}END{print "size is " size}' 
 
 ```
[root@tom ~]#  ls -l | awk 'BEGIN{size=0}{size+=$5}END{print "size is " size}' 

size is 14702

[root@tom ~]# 
 ```
 
 案例二
 
 统计显示/etc/passwd的账号总人数
 
 awk -F ':' 'BEGIN{count=0}$1!~/^$/{count++}END{print "count = "count}' passwd
 
 ```
[root@tom ~]#  awk -F ':' 'BEGIN{count=0}$1!~/^$/{count++}END{print "count = "count}' passwd

count = 23

[root@tom ~]# 

 ```
 
 案例3 统计UID大于100的用户名
 
 awk -F ':' 'BEGIN{count=0}{if ($3 > 100) name[count++]=$1}END{for(i=0;i<count;i++) print i , name[i]}' passwd
 
```
 [root@tom ~]# awk -F ':' 'BEGIN{count=0}{if ($3 > 100) name[count++]=$1}END{for(i=0;i<count;i++) print i , name[i]}' passwd
0 avahi-autoipd
1 systemd-bus-proxy
2 systemd-network
3 polkitd
4 alfred
[root@tom ~]# 

```
 
 
 案例4 
 
 统计netstat -anp状态下为LISTEN和CONNECTED的连接数量
 
 首先查看下netstat -anp的状态
 
 ![](http://p2ehgqigv.bkt.clouddn.com/18-2-5/24322804.jpg)
 
 netstat -anp | awk '$6~/CONNECTED|LISTEN/{sum[$6]++}END{for (i in sum) print i,sum[i]}'
 
 ![](http://p2ehgqigv.bkt.clouddn.com/18-2-5/71338924.jpg)
 
 
 4-8 sed与awk使用比较
 
 awk 和 sed 都可以处理文本
 
 awk 侧重于复杂逻辑处理
 
 sed 侧重于正则处理
 
 
 awk 和sed 可以 共同使用
 
 
 4-9 awk--- 学习总结
 
 ![](http://p2ehgqigv.bkt.clouddn.com/18-2-5/37114214.jpg)

<!--
create time: 2018-02-05 13:56:34
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

