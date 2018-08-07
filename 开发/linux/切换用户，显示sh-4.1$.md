# 切换用户，显示sh-4.1

```
usermod -s /bin/bash 用户名
```

切换用户后，命令提示符显示的是bash-4.1$，有自己的用户目录，这是为什么？

应该是该用户的默认shell不是/bin/bash，而是年代比较久远的shell。比如在执行/bin/sh后的命令提示符是“sh-4.1#”。如果你的用户是Liu_HongYe你可以通过执行 “/bin/bash"临时起用bash 或在root权限下执行 "usermod -s /bin/bash Liu_HongYe" 更改Liu用户的默认shell。看看提示符是不是变成 [Liu_HongYe@localhost ~]$ 了


<!--
create time: 2018-07-12 10:52:15
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

