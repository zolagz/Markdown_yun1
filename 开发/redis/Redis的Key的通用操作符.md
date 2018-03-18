# Redis的Key的通用操作符

[视频教程](https://www.imooc.com/video/14938)

获得所有的key

key * 

查询以my开头的所有的key

keys my?

删除某个知道的值

del my1 my2 my3

查看某个key是否存在  0 不存在，1 存在

exists my1

exists person


重命名key

get company

rename company newcompany

get company

get newcompany


设置过期时间，单位是秒

expire newcompany 1000

通过ttl查看距过期的时间



<!--
create time: 2018-03-14 17:27:43
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

