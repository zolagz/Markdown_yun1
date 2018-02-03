# read命令介绍

###1. 基本读取
read命令接收标准输入（键盘）的输入，或其他文件描述符的输入（后面在说）。得到输入后，read命令将数据放入一个标准变量中。

下面是read命令的最简单形式::

```
#!/bin/bash
echo -n "Enter your name:"   //参数-n的作用是不换行，echo默认是换行
read  name                   //从键盘输入
echo "hello $name,welcome to my program"     //显示信息
exit 0                       //退出shell程序。
```

由于read命令提供了-p参数，允许在read命令行中直接指定一个提示。

所以上面的脚本可以简写成下面的脚本:

```
#!/bin/bash
read -p "Enter your name:" name
echo "hello $name, welcome to my program"
exit 0

```


###从文本读数据

读取ip_info.list文件中的内容

```
#!/bin/bash
count=1
cat ip_info.list | while read line
do
	echo "Line $count:$line"
	count=$[$count + 1]
done
echo "over!!!"
exit 0
```



<!--
create time: 2018-02-03 13:44:35
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

