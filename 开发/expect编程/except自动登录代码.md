# except自动登录代码

```
#!/usr/bin/expect -f
set timeout 30
set host "192.168.1.198"
spawn ssh $host
expect_before "no)?" {
send "yes\r" }
sleep 1
expect "password:"
send "123456\r"
expect "*#"
send "echo my name is fivetrees > /root/fivetrees.txt\r"
interact

```


```

[root@fivetrees ~]# cat expect
#!/usr/bin/expect
for {set i 10} {$i <= 12} {incr i} {
set timeout 30
set ssh_user [lindex $argv 0]
spawn ssh -i .ssh/$ssh_user abc$i.com
expect_before "no)?" {
send "yes\r" }
sleep 1
expect "password*"
send "hello\r"
expect "*#"
send "echo hello expect! > /tmp/expect.txt\r"
expect "*#"
send "echo\r"
}
exit
```



```
#!/usr/bin/expect
if {$argc!=2} {
    send_user "usage: ./expect ssh_user password\n"
    exit
}
foreach i {11 12} {
set timeout 30
set ssh_user [lindex $argv 0]
set password [lindex $argv 1]
spawn ssh -i .ssh/$ssh_user root@xxx.yy.com
expect_before "no)?" {
send "yes\r" }
sleep 1
expect "Enter passphrase for key*"
send "password\r"
expect "*#"
send "echo hello expect! > /tmp/expect.txt\r"
expect "*#"
send "echo\r"
}
exit

```


```

#!/usr/bin/expect
set timeout 20
if {$argc < 1} {
  puts "Usage: script IP"
  exit 1
}
# 替换你自己的用户名
set user "username"
#替换你自己的登录密码
set password "yourpassword"
foreach IP  $argv {
spawn  ssh $user@$IP
expect \
  "(yes/no)?" {
    send "yes\r"
    expect "password:?" {
      send "$password\r"
    }
  } "password:?" {
    send "$password\r"
}
expect "\$?"
# 替换你要执行的命令
send "last\r"
expect "\$?"
sleep 10
send "exit\r"
expect eof
}
使用方法
script_name   ip1  ip2  ip3 ...
```


```

#!/bin/sh
# -*- tcl -*- \
exec tclsh $0 "$@"
package require Expect
set username [lindex $argv 0]
set password [lindex $argv 1]
set argv [lrange $argv 2 end]
set prompt "(%|#|\\$) $"
foreach ip $argv {
    spawn ssh -t $username@$ip sh
    lappend ids $spawn_id
}
expect_before -i ids eof {
    set index [lsearch $ids $expect_out(spawn_id)]
    set ids [lreplace $ids $index $index]
    if [llength $ids] exp_continue
}
expect -i ids "(yes/no)\\?" {
    send -i $expect_out(spawn_id) yes\r
    exp_continue
} -i ids "Enter passphrase for key" {
    send -i $expect_out(spawn_id) \r
    exp_continue
} -i ids "assword:" {
    send -i $expect_out(spawn_id) $password\r
    exp_continue
} -i ids -re $prompt {
    set spawn_id $expect_out(spawn_id)
    send "echo hello; exit\r"
    exp_continue
} timeout {
    exit 1
}

```

<!--
create time: 2018-02-02 19:09:03
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

