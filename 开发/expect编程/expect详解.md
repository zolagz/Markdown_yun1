# expect详解


###背景

linux脚本中有很多场景是进行远程操作的，例如远程登录ssh、远程复制scp、文件传输sftp等。这些命令中都会涉及到安全密码的输入，正常使用命令时是需要人工手动输入密码并接受安全验证的。为了实现自动化远程操作，我们可以借用expect的功能。

expect是一个免费的编程工具语言，用来实现自动和交互式任务进行通信，而无需人的干预。expect是不断发展的，随着时间的流逝，其功能越来越强大，已经成为系统管理员的的一个强大助手。expect需要Tcl编程语言的支持，要在系统上运行expect必须首先安装Tcl。

###expect的安装

expect是在Tcl基础上创建起来的，所以在安装expect前我们应该先安装Tcl。

（一）Tcl 安装

主页: http://www.tcl.tk

下载地址: http://www.tcl.tk/software/tcltk/downloadnow84.tml

1.下载源码包

```
wget http://nchc.dl.sourceforge.net/sourceforge/tcl/tcl8.4.11-src.tar.gz
```
2.解压缩源码包

	tar xfvz tcl8.4.11-src.tar.gz
	
3.安装配置

```
cd tcl8.4.11/unix
./configure --prefix=/usr/tcl --enable-shared 
make
make install
```

注意：

1、安装完毕以后，进入tcl源代码的根目录，把子目录unix下面的tclUnixPort.h copy到子目录generic中。

2、暂时不要删除tcl源代码，因为expect的安装过程还需要用。

（二）expect 安装 (需Tcl的库)

主页: http://expect.nist.gov/

1.下载源码包
	
	wget http://sourceforge.net/projects/expect/files/Expect/5.45/expect5.45.tar.gz/download

2.解压缩源码包

	tar xzvf expect5.45.tar.gz

3.安装配置

```
cd expect5.45 
./configure --prefix=/usr/expect --with-tcl=/usr/tcl/lib --with-tclinclude=../tcl8.4.11/generic
make
make install
ln -s /usr/tcl/bin/expect /usr/expect/bin/expect
```

###expect

expect的核心是spawn、expect、send、set。

spawn 调用要执行的命令

   expect 等待命令提示信息的出现，也就是捕捉用户输入的提示：
   
   send 发送需要交互的值，替代了用户手动输入内容
   
   set 设置变量值
   
   interact 执行完成后保持交互状态，把控制权交给控制台，这个时候就可以手工操作了。如果没有这一句登录完成后会退出，而不是留在远程终端上。
   
   expect eof 这个一定要加，与spawn对应表示捕获终端输出信息终止，类似于if....endif

expect脚本必须以interact或expect eof结束，执行自动化任务通常expect eof就够了。

其他设置

   设置expect永不超时 set timeout -1
   
   设置expect 300秒超时，如果超过300没有expect内容出现，则退出 set timeout 300

###expect编写语法

expect使用的是tcl语法

    一条Tcl命令由空格分割的单词组成. 其中, 第一个单词是命令名称, 其余的是命令参数
    cmd arg arg arg
    $符号代表变量的值. 在本例中, 变量名称是foo.
    $foo
    方括号执行了一个嵌套命令. 例如, 如果你想传递一个命令的结果作为另外一个命令的参数, 那么你使用这个符号
    [cmd arg]
    双引号把词组标记为命令的一个参数. "$"符号和方括号在双引号内仍被解释
    "some stuff"
    大括号也把词组标记为命令的一个参数. 但是, 其他符号在大括号内不被解释
    {some stuff}
    反斜线符号是用来引用特殊符号. 例如：n 代表换行. 反斜线符号也被用来关闭"$"符号, 引号,方括号和大括号的特殊含义

示例

login.exp专用于远程登录，快捷使用方式：

```
login.exp "exclude" "${remote_ip}" "${remote_user}" "${remote_passwd}" "${remote_command}"
```

```

#!/usr/bin/expect -f
##########################################################
# 通过SSH登陆和执行命令
#参数:1.Use_Type [check/execute]
#  2.SSHServerIp
#  3.SSHUser
#  4.SSHPassword
#  5.CommandList [多个命令间以;间隔]
#返回值：
#  0 成功
#  1 参数个数不正确
#  2 SSH 服务器服务没有打开
#  3 SSH 用户密码不正确
#  4 连接SSH服务器超时
##########################################################
proc usage {} {
 regsub ".*/" $::argv0 "" name
 send_user "Usage:\n"
 send_user " $name Use_Type SSHServerIp SSHUser SSHPassword CommandList\n"
 exit 1
} 
## 判断参数个数
if {[llength $argv] != 5} {
 usage
}
#设置变量值
set Use_Type [lindex $argv 0]
set SSHServerIp [lindex $argv 1]
set SSHUser [lindex $argv 2]
set SSHPassword [lindex $argv 3]
set CommandList [lindex $argv 4]
#spawn ping ${SSHServerIp} -w 5
#expect {
# -nocase -re "100% packet loss" {
#  send_error "Ping ${SSHServerIp} is unreachable, Please check the IP address.\n"
#  exit 1
# }
#}
set timeout 360
set resssh 0
#定义变量标记ssh连接时是否输入yes确认
set inputYes 0
set ok_string LOGIN_SUCCESS
if {$Use_Type=="check"} {
 #激活ssh连接，如果要需要输入yes确认，输入yes，设置inputYes为1，否则输入ssh密码
 spawn ssh ${SSHUser}@${SSHServerIp} "echo $ok_string"
} else {   
 spawn ssh ${SSHUser}@${SSHServerIp} "$CommandList"
}
expect {
 -nocase -re "yes/no" {
  send -- "yes\n"
  set inputYes 1
 }
 -nocase -re "assword: " {
  send -- "${SSHPassword}\n"
  set resssh 1
 }
 #-nocase -re "Last login: " { 
 #  send -- "${CommandList}\n"
 #}
 $ok_string {}
 -nocase -re "Connection refused" {
  send_error "SSH services at ${SSHServerIp} is not active.\n"
  exit 2
 }
 timeout {
  send_error "Connect to SSH server ${SSHUser}@${SSHServerIp} timeout(10s).\n"
  exit 4
 }
}
#如果输入了yes确认，输入ssh密码
if {$inputYes==1} {
 expect {
  -nocase -re "assword: " {
   send -- "${SSHPassword}\n"
   set resssh 1
  }
 }
}
#如果出现try again或者password:提示，说明输入的用户密码错误，直接退出。
if {$resssh==1} {
 expect {
  -nocase -re "try again" {
   send_error "SSH user:${SSHUser} passwd error.\n"
   exit 3
  }
  -nocase -re "assword:" {
   send_error "SSH user:${SSHUser} passwd error.\n"
   exit 3
  }
  eof {}
 }
}
send_error -- "$expect_out(buffer)"
#-nocase -re "No such user" {
#  send_error "No such user.\n"
#  exit 5
# }
#exit

```

总结

以上就是这篇文章的全部内容了，希望本文的内容对大家的学习或者工作具有一定的参考学习价值，如果有疑问大家可以留言交流，谢谢大家对脚本之家的支持。

[原文链接](http://www.jb51.net/article/131165.htm)

<!--
create time: 2018-02-02 17:07:48
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

