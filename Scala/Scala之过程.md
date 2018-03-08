# Scala之过程

在Scala中，定义函数时，如果函数体直接包裹在了花括号里面，而没有使用=连接，则函数的返回值类型就是Unit。这样的函数就被称之为过程，即过程就是没有返回值的函数。

过程还有一种写法，就是将函数的返回值类型定义为Unit。

```
def sayHello(name: String) = "Hello, " + name//函数

def sayHello(name: String) { print("Hello, " + name); "Hello, " + name }//有值，但未使用=号，还是过程

def sayHello(name: String): Unit = "Hello, " + name//有值，有=号，但强制返回类型为空，则还是过程

```


<!--
create time: 2018-03-08 19:51:14
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

