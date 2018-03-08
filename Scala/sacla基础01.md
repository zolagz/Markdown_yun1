###scala定义变量

// 使用val定义的变量值是不可改变的，相当于java中用final修饰的变量

val i = 1


// 使用var 定义的变量是可变的，在scalca中鼓励使用var

var s = "Hello World"

// 变量名在前，类型在后

val str : String = "itcast"

scalca和java一样，有7种数值类型：Byte,Char,Short,Int,Long,Float和Double，Boolearn类型（无包装类型）




###scala默认参数

```
scala> def sayHello(name:String,age:Int = 20){
     | println("Hello,"+name + ",your age is "+ age)}
sayHello: (name: String, age: Int)Unit

scala> sayHello("tom")
Hello,tom,your age is 20

scala> sayHello("guan",25)
Hello,guan,your age is 25

scala> sayHello("jasper",50)
Hello,jasper,your age is 50

```