# Scala-面向对象编程

###定义一个简单的类


```
class HelloWorld{
	private var name = "leo"
	
	def sayHello(){
		print("Hello,"+name)
	}	
	
	def getName = name
}


// 创建类的对象，并调用其方法

val hw1 = new HelloWorld

hw1.sayHello()

如果定义方法时不带括号，则调用方法是也不能带括号
print(hw1.getName)


```

<!--
create time: 2018-02-26 12:06:00
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

