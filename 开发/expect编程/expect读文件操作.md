# expect读文件操作


```

#!/usr/bin/expect --

#open a file

set fd [open "/root/guan/myFile.txt" r]
set number 0

#read each line

while { [gets $fd line ] >=0 } {
 puts $line
 incr number 
 }
puts "Number of lines:$number"

close $fd
```

[来自这里](http://blog.csdn.net/wjciayf/article/details/54408819)


shell中通过expect读文件

```
#!/bin/bash

expect <<EOF

set file '/root/guan/myFile.txt'

set fd [open "/root/guan/myFile.txt" r]

set n 0

while { [gets \$fd line] != -1 } {

        incr n
        puts "$content\$n:\$line"
}

close \$fd

EOF
```

**记住:** 如果想用shell变量的地方，就用$，用expect变量的地方，就用\\$


<!--
create time: 2018-02-03 15:53:00
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

