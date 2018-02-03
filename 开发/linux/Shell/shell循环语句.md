# shell循环语句

###for语法格式

```
for var in list;do
　　commands
done
```

 

其中list可以包含:

1) 直接写

```
for alpha in a b c d;do
    echo $alpha
done

```
 

2）变量

```
list="a b c d"
for alpha in $list;do
    echo $alpha
done

```

在shell执行的时候会进行变量替换，上面的list变量替换之后，for循环的形式和1中的形式一模一样。但是如果为$list加上了引号，即如果写为下面的形式:

```

list="a b c d"
for alpha in "$list";do
    echo $list
done

shell变量替换之后为:

list="a b c d"
for alpha in "a b c d";do
    echo $list
done
```
这时输出就只有一行a b c d。

3）shell命令

```
for alpha in `cat alpha.txt`;do
    echo $alpha
done

```
假设alpha.txt文件里面的内容就是a b c d，那么通过使用``符先将文件内容读出，再进行迭代，结果和1一样

4)读取文件目录

```
for file in $HOME/a/*;do
    echo $file
done

```
上面的代码先进行通配符glob扩展，假设目录a下有2个文件1.txt, 2.txt，一个文件夹b，那么经过扩展之后实际为:

```

for file in $HOME/a/1.txt $HOME/a/2.txt $HOME/a/b;do
    echo $file
done

```
此时file的值依次为$HOME/a/1.txt，$HOME/a/2.txt, $HOME/a/b。

但是假设a不是一个目录，是一个文件，此时通配符扩展会失败，此时file的值直接就是$HOME/a/*。

这里还需要注意的一个地方是，这里进行的是shell glob的扩展，因此扩展的时候不能越过文件边界，换句欢说，如果b目录下面还有文件，这些文件是无法扩展出来的，即扩展无法越过文件夹b

 

IFS

for循环当中，list的被如何分割就是由IFS决定的，默认情形下，IFS的值是:

Tab

空格

换行

你可以重新给IFS赋值，:

IFS=: #此时分隔符为:
IFS=:; #此时分隔符为:和;

 

 

C风格的for循环

for (( i = 0; i < 10; i++ ));do

　　commands

done

这里的风格和C中一样，其中的变量i可以是任何变量

 

while 循环

while command;do

　　commands

done

其中的command可以是shell command，也可以test condition。如果command的返回值为0或者测试成立，则执行，否则不执行。

这里需要注意的是，while可以使用多个条件，但是只有最后一个条件起作用:

var=100
while [ $var -lt 0 ];[ $var -gt 0 ];do
    echo $var
done

在这段代码中，虽然第一个条件一开始就不成立，但是起作用的是最后一个条件，因此，这是一个无线循环

 

until循环

until command;do

　　commnds

done

和while一样，唯一不同的是如果command返回0，则不执行，否则就执行


[笔记来源](https://www.cnblogs.com/chaoguo1234/p/5720949.html) 

<!--
create time: 2018-02-03 17:43:19
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

