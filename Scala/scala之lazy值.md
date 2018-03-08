# scala之lazy值



在Scala中，提供了lazy值的特性，也就是说，如果将一个变量声明为lazy，则只有在第一次使用该变量时，变量对应的表达式才会发生计算。这种特性对于特别耗时的计算操作特别有用，比如打开文件进行IO，进行网络IO等。

```

import scala.io.Source._

lazy val lines = fromFile("C://Users//Administrator//Desktop//test.txt").mkString

```
即使文件不存在，也不会报错，只有第一个使用变量时会报错，证明了表达式计算的lazy特性。

```
val lines = fromFile("C://Users//Administrator//Desktop//test.txt").mkString

lazy val lines = fromFile("C://Users//Administrator//Desktop//test.txt").mkString

```

相当于定义了一个方法，只有在调用该方法时才会去执行方法体：

```

def lines = fromFile("C://Users//Administrator//Desktop//test.txt").mkString

```


<!--
create time: 2018-03-08 19:52:29
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

