# Scala语言基础


在scala命令行模式下输入：paste 进入多行编辑模式，然后通过ctrl + d 执行


### Scala与Java的关系Scala与Java的关系是非常紧密的！！因为Scala是基于Java虚拟机，也就是JVM的一门编程语言。所有Scala的代码，都需要经过编译为字节码，然后交由Java虚拟机来运行。所以Scala和Java是可以无缝互操作的。Scala可以任意调用Java的代码。所以Scala与Java的关系是非常非常紧密的。### Scala解释器的使用REPL：Read（取值）-> Evaluation（求值）-> Print（打印）-> Loop（循环）。

scala解释器也被称为REPL，会快速编译scala代码为字节码，然后交给JVM来执行。计算表达式：在scala>命令行内，键入scala代码，解释器会直接返回结果给你。如果你没有指定变量来存放这个值，那么值默认的名称为res，而且会显示结果的数据类型，比如Int、Double、String等等。例如，输入1 + 1，会看到res0: Int = 2内置变量：在后面可以继续使用res这个变量，以及它存放的值。例如，2.0 * res0，返回res1: Double = 4.0例如，"Hi, " + res0，返回res2: String = Hi, 2自动补全：在scala>命令行内，可以使用Tab键进行自动补全。例如，输入res2.to，敲击Tab键，解释器会显示出以下选项，toCharArray，toLowerCase，toString，toUpperCase。因为此时无法判定你需要补全的是哪一个，因此会提供给你所有的选项。例如，输入res2.toU，敲击Tab键，直接会给你补全为res2.toUpperCase。###声明变量声明val变量：可以声明val变量来存放表达式的计算结果。例如，val result = 1 + 1后续这些常量是可以继续使用的，例如，2 * result但是常量声明后，是无法改变它的值的，例如，result = 1，会返回error: reassignment to val的错误信息。声明var变量：如果要声明值可以改变的引用，可以使用var变量。例如，val myresult = 1，myresult = 2但是在scala程序中，通常建议使用val，也就是常量，因此比如类似于spark的大型复杂系统中，需要大量的网络传输数据，如果使用var，可能会担心值被错误的更改。在Java的大型复杂系统的设计和开发中，也使用了类似的特性，我们通常会将传递给其他模块 / 组件 / 服务的对象，设计成不可变类（Immutable Class）。在里面也会使用java的常量定义，比如final，阻止变量的值被改变。从而提高系统的健壮性（robust，鲁棒性），和安全性。指定类型：无论声明val变量，还是声明var变量，都可以手动指定其类型，如果不指定的话，scala会自动根据值，进行类型的推断。例如，val name: String = null例如，val name: Any = "leo"声明多个变量：可以将多个变量放在一起进行声明。   
   例如，val name1, name2:String = null   
   例如，val num1, num2 = 100### 数据类型与操作符·基本数据类型：Byte、Char、Short、Int、Long、Float、Double、Boolean。乍一看与Java的基本数据类型的包装类型相同，但是scala没有基本数据类型与包装类型的概念，统一都是类。scala自己会负责基本数据类型和引用类型的转换操作。    ·使用以上类型，直接就可以调用大量的函数，例如，1.toString()，1.to(10)。·类型的加强版类型：scala使用很多加强类给数据类型增加了上百种增强的功能或函数。    ·例如，String类通过StringOps类增强了大量的函数，"Hello".intersect(" World")。    ·例如，Scala还提供了RichInt、RichDouble、RichChar等类型，RichInt就提供了to函数，1.to(10)，此处Int先隐式转换为RichInt，然后再调用其to函数·基本操作符：scala的算术操作符与java的算术操作符也没有什么区别，比如+、-、*、/、%等，以及&、|、^、>>、<<等。    ·但是，在scala中，这些操作符其实是数据类型的函数，比如1 + 1，可以写做1.+(1)    ·例如，1.to(10)，又可以写做1 to 10    ·scala中没有提供++、--操作符，我们只能使用+和-，比如counter = 1，counter++是错误的，必须写做counter += 1.### 函数调用与apply()函数·函数调用方式：在scala中，函数调用也很简单。    ·例如，import scala.math._，sqrt(2)，pow(2, 4)，min(3, Pi)。    ·不同的一点是，如果调用函数时，不需要传递参数，则scala允许调用函数时省略括号的，例如，"Hello World".distinct·apply函数    ·Scala中的apply函数是非常特殊的一种函数，在Scala的object中，可以声明apply函数。而使用“类名()”的形式，其实就是“类名.apply()”的一种缩写。通常使用这种方式来构造类的对象，而不是使用“new 类名()”的方式。    ·例如，"Hello World"(6)，因为在StringOps类中有def apply(n: Int): Char的函数定义，所以"Hello World"(6)，实际上是"Hello World".apply(6)的缩写。    ·例如，Array(1, 2, 3, 4)，实际上是用Array object的apply()函数来创建Array类的实例，也就是一个数组。

### if表达式if表达式的定义：在Scala中，if表达式是有值的，就是if或者else中最后一行语句返回的值。
   例如，val age = 30; if (age > 18) 1 else 0   
   可以将if表达式赋予一个变量，例如，val isAdult = if (age > 18) 1 else 0   另外一种写法，var isAdult = -1; if(age > 18) isAdult = 1 else isAdult = 0，但是通常使用上一种写法if表达式的类型推断：由于if表达式是有值的，而if和else子句的值类型可能不同，此时if表达式的值是什么类型呢？Scala会自动进行推断，取两个类型的公共父类型。    
   例如，if(age > 18) 1 else 0，表达式的类型是Int，因为1和0都是Int   
   例如，if(age > 18) "adult" else 0，此时if和else的值分别是String和Int，则表达式的值是Any，Any是String和Int的公共父类型   
   如果if后面没有跟else，则默认else的值是Unit，也用()表示，类似于java中的void或者null。例如，val age = 12; if(age > 18) "adult"。此时就相当于if(age > 18) "adult" else ()。将if语句放在多行中：默认情况下，REPL只能解释一行语句，但是if表达式通常需要放在多行。   
   可以使用{}的方式，比如以下方式，或者使用:paste和ctrl+D的方式。if(age > 18) { "adult" } else if(age > 12) "teenager" else "children"### 语句终结符、块表达式·默认情况下，scala不需要语句终结符，默认将每一行作为一个语句·一行放多条语句：如果一行要放多条语句，则必须使用语句终结符    ·例如，使用分号作为语句终结符，var a, b, c = 0; if(a < 10) { b = b + 1; c = c + 1 }    ·通常来说，对于多行语句，还是会使用花括号的方式if(a < 10) {    b = b + 1    c = c + 1}·块表达式：块表达式，指的就是{}中的值，其中可以包含多条语句，最后一个语句的值就是块表达式的返回值。    ·例如，var d = if(a < 10) { b = b + 1; c + 1 }### 输入和输出·print和println：print打印时不会加换行符，而println打印时会加一个换行符。    ·例如，print("Hello World"); println("Hello World")·printf：printf可以用于进行格式化    ·例如，printf("Hi, my name is %s, I'm %d years old.\n", "Leo", 30)·readLine: readLine允许我们从控制台读取用户输入的数据，类似于java中的System.in和Scanner的作用。·综合案例：游戏厅门禁val name = readLine("Welcome to Game House. Please tell me your name: ")print("Thanks. Then please tell me your age: ")val age = readInt()if(age > 18) {  printf("Hi, %s, you are %d years old, so you are legel to come here!", name, age)} else {  printf("Sorry, boy, %s, you are only %d years old. you are illegal to come here!", name, age)}###循环·while do循环：Scala有while do循环，基本语义与Java相同。var n = 10while(n > 0) {  println(n)  n -= 1}·Scala没有for循环，只能使用while替代for循环，或者使用简易版的for语句    ·简易版for语句：var n = 10; for(i <- 1 to n) println(i)    ·或者使用until，表式不达到上限：for(i <- 1 until n) println(i)    ·也可以对字符串进行遍历，类似于java的增强for循环，for(c <- "Hello World") print(c)·跳出循环语句    ·scala没有提供类似于java的break语句。    ·但是可以使用boolean类型变量、return或者Breaks的break函数来替代使用。import scala.util.control.Breaks._breakable {    var n = 10    for(c <- "Hello World") {        if(n == 5) break;        print(c)        n -= 1    }}###循环

·while do循环：Scala有while do循环，基本语义与Java相同。var n = 10while(n > 0) {  println(n)  n -= 1}·Scala没有for循环，只能使用while替代for循环，或者使用简易版的for语句    ·简易版for语句：var n = 10; for(i <- 1 to n) println(i)    ·或者使用until，表式不达到上限：for(i <- 1 until n) println(i)    ·也可以对字符串进行遍历，类似于java的增强for循环，for(c <- "Hello World") print(c)·跳出循环语句    ·scala没有提供类似于java的break语句。    ·但是可以使用boolean类型变量、return或者Breaks的break函数来替代使用。import scala.util.control.Breaks._breakable {    var n = 10    for(c <- "Hello World") {        if(n == 5) break;        print(c)        n -= 1    }}### 高级for循环·多重for循环：九九乘法表for(i <- 1 to 9; j <- 1 to 9) {  if(j == 9) {    println(i * j)  } else {    print(i * j + " ")  }}·if守卫：取偶数for(i <- 1 to 100 if i % 2 == 0) println(i)·for推导式：构造集合for(i <- 1 to 10) yield i


### 函数的定义与调用在Scala中定义函数时，需要定义函数的函数名、参数、函数体。我们的第一个函数如下所示：
```
def sayHello(name: String, age: Int) = {  if (age > 18) { printf("hi %s, you are a big boy\n", name); age }   else { printf("hi %s, you are a little boy\n", name); age }sayHello("leo", 30)

```Scala要求必须给出所有参数的类型，但是不一定给出函数返回值的类型，只要右侧的函数体中不包含递归的语句，Scala就可以自己根据右侧的表达式推断出返回类型。### 在代码块中定义包含多行语句的函数体单行的函数：

def sayHello(name: String) = print("Hello, " + name)如果函数体中有多行代码，则可以使用代码块的方式包裹多行代码，代码块中最后一行的返回值就是整个函数的返回值。与Java中不同，不是使用return返回值的。比如如下的函数，实现累加的功能：```
def sum(n: Int) = {  var sum = 0;  for(i <- 1 to n) sum += i  sum}
```

### 递归函数与返回类型如果在函数体内递归调用函数自身，则必须手动给出函数的返回类型。例如，实现经典的斐波那契数列：

```
9 + 8; 8 + 7 + 7 + 6; 7 + 6 + 6 + 5 + 6 + 5 + 5 + 4; ....def fab(n: Int): Int = {  if(n <= 1) 1  else fab(n - 1) + fab(n - 2)}
```

###默认参数

在Scala中，有时我们调用某些函数时，不希望给出参数的具体值，而希望使用参数自身默认的值，此时就定义在定义函数时使用默认参数。def sayHello(firstName: String, middleName: String = "William", lastName: String = "Croft") = firstName + " " + middleName + " " + lastName 如果给出的参数不够，则会从作往右依次应用参数。

###带名参数

在调用函数时，也可以不按照函数定义的参数顺序来传递参数，而是使用带名参数的方式来传递。sayHello(firstName = "Mick", lastName = "Nina", middleName = "Jack") 还可以混合使用未命名参数和带名参数，但是未命名参数必须排在带名参数前面。sayHello("Mick", lastName = "Nina", middleName = "Jack")


<!--
create time: 2018-02-26 10:42:46
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

