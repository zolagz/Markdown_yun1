# expect入门1


### 一个例子

```
#!/usr/bin/expect -f 
set ip [lindex $argv 0 ]  //接收第一个参数,并设置IP 
set password [lindex $argv 1 ] //接收第二个参数,并设置密码 
set timeout 10     //设置超时时间 
spawn ssh root@$ip  //发送ssh请滶 
expect {     //返回信息匹配 
 "*yes/no" { send "yes\r"; exp_continue} //第一次ssh连接会提示yes/no,继续 
 "*password:" { send "$password\r" }  //出现密码提示,发送密码 
} 
interact   //交互模式,用户会停留在远程服务器上面.

```

运行结构

```
root@ubuntu:/home/zhangy# ./test.exp 192.168.1.130 admin 
spawn ssh root@192.168.1.130 
Last login: Fri Sep 7 10:47:43 2012 from 192.168.1.142 
[root@linux ~]#
```

[参考文章](http://www.jb51.net/article/103075.htm)

----------------------------------



常用的有3个命令：

expect：从进程接收字符串

send：用于向进程发送字符串

spawn：启动新的进程

下面是一个简单的自动执行 scp 命令的例子，新建一个文件 scp.exp ，内容如下：

```
#!/usr/bin/expect
set timeout 10
set host [lindex $argv 0]
set username [lindex $argv 1]
set password [lindex $argv 2]
set src_file [lindex $argv 3]
set dest_file [lindex $argv 4]
spawn scp $src_file $username@$host:$dest_file
expect {
 "*password:"
 {
 send "$password\n"
 }
}

```


第1行的意思是告诉操作系统脚本里的代码使用那一个shell来执行，#!后面跟着的就是 expect 的二进制文件路径，所以必须将它放在文件的第1行。

第2行 set timeout 是用于设置执行命令的超时时间，单位是秒，-1表示永不超时。这里的 set 语句就是 shell 里设置变量的意思。

第3到7行也是设置变量值，不同的是先定义了一个变量，然后变量的值是从文件入参中获取的。

expect 脚本可以接受从 bash 传递过来的参数，用[lindex $argv n]获得，n从0始，分别表示第1个，第2个，第3个....参数

第8行的 spawn 是 expect 的内部命令，这里给 scp 的运行进程加个壳，用来传递交互指令。

第9行的 expect 也是 expect 的内部命令，后面接的大括号包含的语句块，其中 "password:"的意思是判断第8行的 spawn 执行后的输出结果里是否包含以password:结尾的字符串，前面的是正则表达式，表示匹配任意字符串。如果有则立即返回，否则就等待一段时间后返回，这里等待时长就是前面设置的10秒 。


第12行的 send 同样是内部命令，意思是执行交互动作，相当于人工输入密码，密码就是上面定义的变量 password ，也即文件执行时的第3个参数的值。

最后将该文件保存，并赋予执行的权限：
chmod +x scp.exp

这样执行
./scp.expect 主机地址 用户名 密码 源文件路径 目标文件路径
即可自动执行上传文件功能，无需人工输入密码。

由此类推可以将原先需要人工介入的命令操作嵌入到 shell 脚本中，实现命令操作的自动化。


[来源](https://www.jianshu.com/p/b9cd24c191dc)


<!--
create time: 2018-02-02 16:33:26
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

