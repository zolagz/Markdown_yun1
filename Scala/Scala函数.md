# Scala函数

在Scala中定义函数时，需要定义函数的函数名、参数、函数体。

我们的第一个函数如下所示：

```
def sayHello(name: String, age: Int) = {

  if (age > 18) { printf("hi %s, you are a big boy\n", name); age }
  else { printf("hi %s, you are a little boy\n", name); age
}

##调用方式

sayHello("leo", 30)

```

Scala要求必须给出所有参数的类型，但是不一定给出函数返回值的类型，只要右侧的函数体中不包含递归的语句，Scala就可以自己根据右侧的表达式推断出返回类型。

###在代码块中定义包含多行语句的函数体

单行的函数：

	def sayHello(name: String) = print("Hello, " + name)

如果函数体中有多行代码，则可以使用代码块的方式包裹多行代码，代码块中最后一行的返回值就是整个函数的返回值。与Java中不同，不是使用return返回值的。

比如如下的函数，实现累加的功能：

```
def sum(n: Int) = {
  var sum = 0;
  for(i <- 1 to n) sum += i
  sum
}

```

###递归函数与返回类型

如果在函数体内递归调用函数自身，则必须手动给出函数的返回类型。

例如，实现经典的斐波那契数列：

```
9 + 8; 8 + 7 + 7 + 6; 7 + 6 + 6 + 5 + 6 + 5 + 5 + 4; ....

def fab(n: Int): Int = {
  if(n <= 1) 1
  else fab(n - 1) + fab(n - 2)
}
```

###默认参数

在Scala中，有时我们调用某些函数时，不希望给出参数的具体值，而希望使用参数自身默认的值，此时就定义在定义函数时使用默认参数。

```
def sayHello(firstName: String, middleName: String = "William", lastName: String = "Croft") = firstName + " " + middleName + " " + lastName

```

如果给出的参数不够，则会从左往右依次应用默认参数。

```
def sayHello(name: String, age: Int = 20) {
  print("Hello, " + name + ", your age is " + age)
}
<!--调用方式-->
sayHello("leo")
```

###函数调用时带名参数

在调用函数时，也可以不按照函数定义的参数顺序来传递参数，而是使用带名参数的方式来传递。

```
sayHello(firstName = "Mick", lastName = "Nina", middleName = "Jack")
```

还可以混合使用未命名参数和带名参数，但是未命名参数必须排在带名参数前面。

```
sayHello("Mick", lastName = "Nina", middleName = "Jack")
```

###变长参数

在Scala中，有时我们需要将函数定义为参数个数可变的形式，则此时可以使用变长参数定义函数。

```
def sum(nums: Int*) = {
  var res = 0
  for (num <- nums) res += num
  res
}

<!--调用函数-->
sum(1, 2, 3, 4, 5)
```

<!--
create time: 2018-03-08 19:44:15
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

