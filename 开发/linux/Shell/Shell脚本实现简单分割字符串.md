# Shell脚本实现简单分割字符串

我们有这样一个字符串：

代码如下:

	info='abcd;efgh'

现在想获取abcd和efgh，我们可以简单地用cut工具来获取：

代码如下:

	fstr=`echo $info | cut -d \; -f 1`
	sstr=`echo $info | cut -d \; -f 2`

这里主要是用了cut工具的-d和-f参数：

-d：指定字段的分隔符，默认的字段分隔符为“TAB”；

-f：显示指定字段的内容；

[原文链接](http://www.jb51.net/article/60904.htm)

[cut命令参考](http://www.jb51.net/article/41872.htm)


<!--
create time: 2018-02-02 15:07:51
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

