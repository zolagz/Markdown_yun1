# Scala语法基础2

###变长参数在Scala中，有时我们需要将函数定义为参数个数可变的形式，则此时可以使用变长参数定义函数。```def sum(nums: Int*) = {  var res = 0  for (num <- nums) res += num  res}sum(1, 2, 3, 4, 5)
```###使用序列调用变长参数在如果想要将一个已有的序列直接调用变长参数函数，是不对的。比如val s = sum(1 to 5)。此时需要使用Scala特殊的语法将参数定义为序列，让Scala解释器能够识别。这种语法非常有用！一定要好好主意，在spark的源码中大量地使用到了。val s = sum(1 to 5: _*)案例：使用递归函数实现累加```
def sum2(nums: Int*): Int = {  if (nums.length == 0) 0  else nums.head + sum2(nums.tail: _*)}
```### 过程在Scala中，定义函数时，如果函数体直接包裹在了花括号里面，而没有使用=连接，则函数的返回值类型就是Unit。这样的函数就被称之为过程。过程通常用于不需要返回值的函数。过程还有一种写法，就是将函数的返回值类型定义为Unit。```
def sayHello(name: String) = "Hello, " + namedef sayHello(name: String) { print("Hello, " + name); "Hello, " + name }def sayHello(name: String): Unit = "Hello, " + name```

### lazy值在Scala中，提供了lazy值的特性，也就是说，如果将一个变量声明为lazy，则只有在第一次使用该变量时，变量对应的表达式才会发生计算。这种特性对于特别耗时的计算操作特别有用，比如打开文件进行IO，进行网络IO等。import scala.io.Source._lazy val lines = fromFile("C://Users//Administrator//Desktop//test.txt").mkString即使文件不存在，也不会报错，只有第一个使用变量时会报错，证明了表达式计算的lazy特性。```
val lines = fromFile("C://Users//Administrator//Desktop//test.txt").mkStringlazy val lines = fromFile("C://Users//Administrator//Desktop//test.txt").mkStringdef lines = fromFile("C://Users//Administrator//Desktop//test.txt").mkString```###异常

在Scala中，异常处理和捕获机制与Java是非常相似的。try {  throw new IllegalArgumentException("x should not be negative")} catch {  case _: IllegalArgumentException => println("Illegal Argument!")} finally {  print("release resources!")}try {  throw new IOException("user defined exception")} catch {  case e1: IllegalArgumentException => println("illegal argument")  case e2: IOException => println("io exception")}


<!--
create time: 2018-02-26 11:52:10
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

