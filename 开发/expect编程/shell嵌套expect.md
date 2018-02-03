# shell嵌套expect


```
    #!/bin/bash
    hostname=$1                #接收第一个参数
    password=$2
    /usr/bin/expect <<-EOF    # 重定向到expect
    spawn ssh root@${hostname}
    expect {
            "(yes/no)"
            {
            send "yes\r"
            expect "*assword:" {send "$password\r"}
            }
            "*assword"
            {
            send "$password\r"
            }
    }
    expect "*]#"
    send "echo 'export MAVEN_HOME=/usr/local/maven' >> /etc/profile\r"
    send "echo 'export M2_HOME=\\\$MAVEN_HOME' >> /etc/profile\r"
    send "echo 'export PATH=\\\$PATH:\\\$MAVEN_HOME/bin' >> /etc/profile\r"
    expect "*]#"
    send "source /etc/profile\r"
    expect "*]#"
    send "exit\r"
    expect eof
    EOF  
```
[来自这里](http://blog.csdn.net/snow_114/article/details/53245466)


[小案例](http://blog.csdn.net/ace_fei/article/details/7743576)

<!--
create time: 2018-02-03 15:28:05
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

