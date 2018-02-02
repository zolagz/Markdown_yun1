# 用expect实现ssh自动登录服务器并进行批量管理的实现方法


```

#!/usr/local/bin/expect
set PASSWD [lindex $argv 1]
set IP     [lindex $argv 0]
set CMD [lindex $argv 2]
spawn ssh $IP $CMD
expect "(yes/no)?" {
send "yesr"
expect "password:"
send "$PASSWDr"
} "password:" {send "$PASSWDr"} "*host " {exit 1}
expect eof


```

注解：

第一行,制定使用/usr/local/bin目录下的expect命令对后面的程序进行解释。

第二行，三行，四行，分别从命令行参数中获取要登录的主机IP地址，登陆密码，以及要执行的命令。

第五行，大概就是要触发这样一个事件，执行ssh $IP $CMD命令。

第6行至第11行就是expect的整个交互过程了。

如果读取到(yes/no)?提示符，就输入yes并回车，如果读取到password：提示输入密码的字符串，就输入用户登录密码(root用户)。

当然如果不是第一次登陆，以前已经登录过的话，当输入ssh $IP $CMD回车后，会直接提示输入密码也就是说会读到字符串”* password:”,这个时候会输入密码回车(send "$PASSWDr").

另外，如果主机不可达的话，(yes/no)?和”password:”的可能都不会出现，系统会提示:

“No route to host”退出程序。




<!--
create time: 2018-02-02 19:11:50
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

