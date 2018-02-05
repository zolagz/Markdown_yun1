# 3-1 sed介绍

课程链接：https://www.imooc.com/video/14484

用处：

*  自动处理文件
*  分析日子文件
*  修改配置文件 

###sed的学习内容

sed 是如何进行文本处理的

![](http://p2ehgqigv.bkt.clouddn.com/18-2-4/37143046.jpg)

sed---行处理
*	sed 一次处理一行内容
*  sed  不改变文本内容（除非重定向）

sed的基本操作命令
	
* 文本行处理 （增，删，替）
* 字符串替换（取用）

sed的高级操作命令



------------


使用sed的格式
* 命令行格式

	$ sed [options] 'command' file(s)

	option : -e ; -n
	
	command:行定位（正则） + sed命令（操作）

* 脚本格式
	
```
	$ sed -f scriptfile file(s)
```
	

基本命令  p  打印  （需要与-n同时使用）

sed 'p' passwd


执行结果，每一行输出了两次

sed 输出一次，p 又输出一次

sed -n 'p' passwd

输出一次	

###sed  -- 行定位

####定位-行：  x; /pattern/

x代表行号
pattern  正则，两边要加/ 


例子 

	[root@tom ~]# nl passwd | sed -n '10p'
    10  operator:x:11:0:operator:/root:/sbin/nologin

nl  应该是number of line  ,行号；这里为了打印行号用了nl

	[root@tom ~]# sed -n '/alfred/p' passwd 
	alfred:x:1000:1000:alfred:/home/alfred:/bin/bash

####定位几行 ： x,y;/patter/,x;

/pattern1/,/pattern2/;

x,y! 取反的操作

定位间隔几行：first~step


从第x行到第y行，满足条件的


nl passwd | sed -n '10,20p' 

```
[root@tom ~]# nl passwd | sed -n '10,20p' 
    10  operator:x:11:0:operator:/root:/sbin/nologin
    11  games:x:12:100:games:/usr/games:/sbin/nologin
    12  ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    13  nobody:x:99:99:Nobody:/:/sbin/nologin
    14  avahi-autoipd:x:170:170:Avahi IPv4LL Stack:/var/lib/avahi-autoipd:/sbin/nologin
    15  systemd-bus-proxy:x:999:997:systemd Bus Proxy:/:/sbin/nologin
    16  systemd-network:x:998:996:systemd Network Management:/:/sbin/nologin
    17  dbus:x:81:81:System message bus:/:/sbin/nologin
    18  polkitd:x:997:995:User for polkitd:/:/sbin/nologin
    19  tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
    20  postfix:x:89:89::/var/spool/postfix:/sbin/nologin
[root@tom ~]# 

```

利用正则 

nl passwd | sed -n '/opera/,/alfred/p'

```
[root@tom ~]# nl passwd | sed -n '/opera/,/alfred/p'
    10  operator:x:11:0:operator:/root:/sbin/nologin
    11  games:x:12:100:games:/usr/games:/sbin/nologin
    12  ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    13  nobody:x:99:99:Nobody:/:/sbin/nologin
    14  avahi-autoipd:x:170:170:Avahi IPv4LL Stack:/var/lib/avahi-autoipd:/sbin/nologin
    15  systemd-bus-proxy:x:999:997:systemd Bus Proxy:/:/sbin/nologin
    16  systemd-network:x:998:996:systemd Network Management:/:/sbin/nologin
    17  dbus:x:81:81:System message bus:/:/sbin/nologin
    18  polkitd:x:997:995:User for polkitd:/:/sbin/nologin
    19  tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
    20  postfix:x:89:89::/var/spool/postfix:/sbin/nologin
    21  sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
    22  alfred:x:1000:1000:alfred:/home/alfred:/bin/bash
[root@tom ~]# 
```
不选择第10行

nl passwd | sed -n '10!p'

```
[root@tom ~]# nl passwd | sed -n '10!p'
     1  root:x:0:0:root:/root:/bin/bash
     2  bin:x:1:1:bin:/bin:/sbin/nologin
     3  daemon:x:2:2:daemon:/sbin:/sbin/nologin
     4  adm:x:3:4:adm:/var/adm:/sbin/nologin
     5  lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
     6  sync:x:5:0:sync:/sbin:/bin/sync
     7  shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
     8  halt:x:7:0:halt:/sbin:/sbin/halt
     9  mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    11  games:x:12:100:games:/usr/games:/sbin/nologin
    12  ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
    13  nobody:x:99:99:Nobody:/:/sbin/nologin
    14  avahi-autoipd:x:170:170:Avahi IPv4LL Stack:/var/lib/avahi-autoipd:/sbin/nologin
    15  systemd-bus-proxy:x:999:997:systemd Bus Proxy:/:/sbin/nologin
    16  systemd-network:x:998:996:systemd Network Management:/:/sbin/nologin
    17  dbus:x:81:81:System message bus:/:/sbin/nologin
    18  polkitd:x:997:995:User for polkitd:/:/sbin/nologin
    19  tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
    20  postfix:x:89:89::/var/spool/postfix:/sbin/nologin
    21  sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
    22  alfred:x:1000:1000:alfred:/home/alfred:/bin/bash
    23  apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
[root@tom ~]# 
```
不包含10到20行之间的内容

nl passwd | sed -n '10,20!p'

```
[root@tom ~]# nl passwd | sed -n '10,20!p'
     1  root:x:0:0:root:/root:/bin/bash
     2  bin:x:1:1:bin:/bin:/sbin/nologin
     3  daemon:x:2:2:daemon:/sbin:/sbin/nologin
     4  adm:x:3:4:adm:/var/adm:/sbin/nologin
     5  lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
     6  sync:x:5:0:sync:/sbin:/bin/sync
     7  shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
     8  halt:x:7:0:halt:/sbin:/sbin/halt
     9  mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    21  sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
    22  alfred:x:1000:1000:alfred:/home/alfred:/bin/bash
    23  apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin
[root@tom ~]# 
```

定位间隔几行：first~step

first : 启始的行号，
step ：步长

nl passwd | sed -n '1~2p'

```
[root@tom ~]#  nl passwd | sed -n '1~2p'
     1  root:x:0:0:root:/root:/bin/bash
     3  daemon:x:2:2:daemon:/sbin:/sbin/nologin
     5  lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
     7  shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
     9  mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
    11  games:x:12:100:games:/usr/games:/sbin/nologin
    13  nobody:x:99:99:Nobody:/:/sbin/nologin
    15  systemd-bus-proxy:x:999:997:systemd Bus Proxy:/:/sbin/nologin
    17  dbus:x:81:81:System message bus:/:/sbin/nologin
    19  tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon:/dev/null:/sbin/nologin
    21  sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
    23  apache:x:48:48:Apache:/usr/share/httpd:/sbin/nologin 
```



<!--
create time: 2018-02-04 22:24:24
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

