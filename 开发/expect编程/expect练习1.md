# expect练习1

批量操作服务器

```
#!/bin/bash
passwd='hadoop'

for item in node1 node2 node3;do
        /usr/bin/expect <<-EOF
        set timeout -1
        spawn ssh root@$item
        expect {
                "*yes/no*" {send "yes\r";exp_continue}
                "*password*" {send "$passwd\r"}
        }
        expect "*#"

        send "cd /root/guan\r"
        expect "*#"
        send "echo abcdefdsafa 113223243pz >> abc.md\r"
        expect "*#"
        send "exit\r"
        interact
        expect eof
        EOF
done

```

批量安装expect

```
#!/bin/bash

passwd = 'hadoop'

for item in node1 node2 node3;
do
    /usr/bin / expect << -EOF
set timeout - 1
spawn ssh root @$item
expect {
    "*yes/no*" { send "yes\r";
        exp_continue }
    "*password*" { send "$passwd\r" }
}

expect "*#"

send "yum install -y expect"

send "exit\r"
interact
expect eof
EOF
done

```


批量下载

```
#!/bin/bash
passwd='hadoop'

for item in node1 node2 node3;do
        /usr/bin/expect <<-EOF
        set timeout -1
        spawn ssh root@$item
        expect {
                "*yes/no*" {send "yes\r";exp_continue}
                "*password*" {send "$passwd\r"}
        }
		 expect "*#"
		 send "mkdir -p /root/temp\r"
		 expect "*#"
		 send "wget  wget http://192.168.25.120/soft/jdk-7u75-linux-x64.tar.gz -P /root/temp\r"

		 expect "*#"

		 send "cd /root/temp\r"
		 expect "*#"
		 send "tar xzvf jdk-7*\r"
		 expect "*#"
        send "exit\r"
        interact
        expect eof
        EOF
done

```



测试可用代码

批量从服务器上下载文件并解压（✨✨⭐️）

```
#!/bin/bash
passwd='hadoop'

for item in node1 node2 node3;do
        /usr/bin/expect <<-EOF
        set timeout -1
        spawn ssh root@$item
        expect {
                "*yes/no*" {send "yes\r";exp_continue}
                "*password*" {send "$passwd\r"}
        }
        expect "*#*"
        send "yum install -y wget\r"
        expect "*#*"

        send "mkdir -p /root/temp\r"
        expect "*#*"
        send "wget  wget http://192.168.25.120/soft/jdk-7u75-linux-x64.tar.gz -P /root/temp\r"

        expect "*#"

        send "cd /root/temp\r"
        expect "*#"
        send "tar xzvf jdk-7*\r"
        expect "*#"
        send "exit\r"
        interact
        expect eof
        EOF
done
```

批量关闭tomcat脚本

```
#!/bin/bash
passwd='hadoop'

for item in node1 node2 node3;do
        /usr/bin/expect <<-EOF
        set timeout -1
        spawn ssh root@$item
        expect {
                "*yes/no*" {send "yes\r";exp_continue}
                "*password*" {send "$passwd\r"}
        }

        expect "*#*"
        send "cd /root/temp/apa*/bin\r"

        expect "*#*"
        send "./shutdown.sh\r"

        expect "*#"
        send "cd /root/temp\r"
        
        expect "*#"
        send "exit\r"
        interact
        expect eof
        EOF
done
```


<!--
create time: 2018-02-03 18:25:46
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

