# shell遍历文件

有一个文件ip_ifno.plist

1.使用read命令读取一行数据

```
while read myline
do
 echo "LINE:"$myline
done < ip_info.plist
```

 2、使用read命令读取一行数据
 
 ```
 cat datafile.txt | while read myline
do
 echo "LINE:"$myline
done
 ```
 3、#读取一行数据
 
 ```
 cat datafile.txt | while myline=$(line)
do
 echo "LINE:"$myline
done
 ```

4、#读取一行数据

```
while myline=$(line)
do
 echo "LINE:"$myline
done < datafile.txt

```

5、#使用read命令读取变量数据

```
cat datafile.txt | while read paraa parab parac
do
 echo "PARAA:"$paraa
 echo "PARAB:"$parab
 echo "PARAC:"$parac
done
```

 6、#使用read命令读取变量数据
 
 ```
 while read paraa parab parac
do
 echo "PARAA:"$paraa
 echo "PARAB:"$parab
 echo "PARAC:"$parac
done < datafile.txt
 ```


```

#!/bin/bash
#version:1.0 
#date:2011年 12月 14日 星期三 21:18:18 CST 
while read myline
do
 echo "LINE:"$myline
 info=$myline
 
fstr=`echo $info | cut -d \; -f 1`
sstr=`echo $info | cut -d \; -f 2`

echo "t1"$fstr
echo "t2"$sstr

done < ip_info.list
```


[读取参考](http://www.jb51.net/article/60904.htm)

<!--

create time: 2018-02-02 23:59:40
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

