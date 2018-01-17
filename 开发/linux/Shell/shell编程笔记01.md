#shell脚本基础

###脚本第一行必须是

/#!/bin/bash

###shell注释一行：
\#开头

###运行shell 脚本：
sh script.sh

or

chmod a+x script.sh

./script.sh


##echo
用于打印输出

echo "welcome to bash"

echo welcome to bash


###定义变量
```
#!/bin/bash
# 在这里定义一个变量
#注意：等号两边是不允许有空格的
a=123
b="Hello World!"
name=Apple
#输出变量(引用变量)
echo "$a"
echo "$b"
echo "this is my name :$name"
```
###Shell常用系统变量解析
> $0 当前程序的名称
> 
> $n 当前程序的第n个参数，n=1，2，...9
> 
> $* 当前程序的所有参数 （不包含程序本身）
> 
> $# 当前程序的参数个数（不包含程序本身）
> 
> $? 命令或程序执行完成后的状态 一般返回0表示执行成功
> 
> $UID 当前用户的ID
> 
> $PWD 当前所在目录

```
#!/bin/bash
#测试一下
echo -e '\033[32m---------\033[0m'
echo "this is $0 param!"
echo "this \$1 is $1 param!"
echo "this \$2 is $2 param!"
echo -e '\033[32m---------\033[0m'
echo "this \$* is $* param!"
echo "this \$# is $# param!"
echo "this \$? is $? param!"
echo
```

###if条件判断语句
>if(表达式);then
>
>	语句1
>
>else
>
>	语句2
>
>fi

```
#!/bin/bash
#测试数字大小
NUM=100
if(($NUM>4));then
	echo "this $NUM is greater 4!"
else
	echo "this $NUM is littler 4!"
fi
```
*案例2 判断目录是否存在，不存在则新建（注意：中括号之间必须要空格）

```
#!/bin/bash
#判断目录是否存在
if [ ! -d /data/2017/09 ];then
	mkdir -p /data/2017/09
else
	echo "This Dir is exit,Plaese exit..."
fi
```
>逻辑运算符解析
>
>-f  判断文件是否存在  eg: if [ -f fileName]
>
>-d  判断目录是否存在  eg: if [ -d dir  ]
>
>-eq  等于  应用于：整型比较
>
>-ne  不等于 应用于：整型比较
>
>-lt 小于 应用于：整型比较
>
>-gt 大于 应用于：整型比较
>
>-ge 大于或等于 应用于：整型比较
>
>-a 双方都成立（and） 逻辑表达式 -a 逻辑表达式
>
>-o 单方成立 （or） 逻辑表达式 -o 逻辑表达式
>
>-z 空字符串

###for循环语句
>for 变量 in 字符串
>
>do
>
>语句1
>
>done

*案例1 打印seq数字循环

```
#!/bin/bash
for i in `sql 15`

do
	
	echo "NUM is $i"
	
done
```

*案例2 求和1 - 100的值

```
sum=0
for((i=1;i<=100;i++))
do
	sum=`expr $i + $sum`
done
echo "sum = $sum"

```

*案例3 找到相关log，然后批量打包

```
#!/bin/bash

for i in 'find/var/log -name "*.log"'
do
	tar -czvf 201709log.tgz $i
done
```

###while的基本格式

```
while test command
do
other commands
done
```

案例1

```
#!/bin/bash
# while command test

var1=10
while [ $var1 -gt 0 ]
do
	echo $var1
	var1=$[ $var1 - 1 ]
done

```

###until命令
until命令刚好与while命令相反


###获取用户输入
1. read  接受标准输入（键盘）的输入；基本读取  

```
#!/bin/bash
#testing the read command

echo -n "Enter you name:"
read name
echo "Hello $name, welcome to my program."
```

-n选项：抑制字符串结尾的新行符，允许脚本用户在字符串后面立即输入数据，而不是在下一行输入。这使得脚本看起来更整齐。

-p 选项；允许在read命令行中直接指定一个提示：

```
#!/bin/bash
#testing the read -p option

read -p "Please enter your age:" age
day=$[ $age * 365 ]
echo "That makes you over $days days old!"

```



