# Linux下使用Shell脚本实现ftp的自动上传下载的代码小结



1. ftp自动登录批量下载文件。

复制代码 代码如下:

```
#####从ftp服务器上的/home/data 到 本地的/home/databackup####
#!/bin/bash
ftp -n<<!
open 192.168.1.171
user guest 123456
binary
cd /home/data
lcd /home/databackup
prompt
mget *
close
bye
!
```

2. ftp自动登录上传文件。

复制代码 代码如下:

```
####本地的/home/databackup to ftp服务器上的/home/data####
#!/bin/bash
ftp -n<<!
open 192.168.1.171
user guest 123456
binary
hash
cd /home/data
lcd /home/databackup
prompt
mput *
close
bye
!
```

3. ftp自动登录下载单个文件。

复制代码 代码如下:

```
####ftp服务器上下载/home/data/a.sh to local /home/databackup####
#!/bin/bash
ftp -n<<!
open 192.168.1.171
user guest 123456
binary
cd /home/data
lcd /home/databackup
prompt
get a.sh a.sh
close
bye
!

```
4. ftp自动登录上传单个文件。

复制代码 代码如下:

```

####把本地/home/databachup/a.sh up ftp /home/databackup 下####
#!/bin/bash
ftp -n<<!
open 192.168.1.171
user guest 123456
binary
cd /home/data
lcd /home/databackup
prompt
put a.sh a.sh
close
bye
!

```

小结：把以上脚本另存为文件加入到crontab中即可实现ftp自动上传、下载文件。
注解：
1. -n 不受.netrc文件的影响。（ftp默认为读取.netrc文件中的设定）
2. << 是使用即时文件重定向输入。
3. !是即时文件的标志它必须成对出现，以标识即时文件的开始和结尾。

<!--
create time: 2018-02-02 19:13:53
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

