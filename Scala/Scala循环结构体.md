# Scala结构体函数

### while do循环：
Scala有while do循环，基本语义与Java相同。

```
var n = 10
while(n > 0) {
  println(n)
  n -= 1
}
```
Scala没有for循环，只能使用while替代for循环，或者使用简易版的for语句

   简易版for语句：
   
   	var n = 10; for(i <- 1 to n) println(i)

  或者使用until，表式不达到上限：
  	
  	for(i <- 1 until n) println(i)

 也可以对字符串进行遍历，类似于java的增强for循环，
 	
 		for(c <- "Hello World") print(c)


###跳出循环语句

scala没有提供类似于java的break语句。但是可以使用boolean类型变量、return或者Breaks的break函数来替代使用。

```
import scala.util.control.Breaks._

breakable {
    var n = 10
    for(c <- "Hello World") {
        if(n == 5) break;
        print(c)
        n -= 1
    }
}

```

###高级for循环：

多次for循环：九九乘法表

```
for(i <- 1 to 9; j <- 1 to 9) {
  if(j == 9) {
    println(i * j)
  } else {
    print(i * j + " ")
  }
}
```
###if守卫：取偶数

	for(i <- 1 to 100 if i % 2 == 0) println(i)


###for推导式：构造集合

	for(i <- 1 to 10) yield i



<!--
create time: 2018-03-08 19:38:39
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

