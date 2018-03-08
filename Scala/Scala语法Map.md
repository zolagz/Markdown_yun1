# Scala语法Map


### 创建一个不可变的Map

```
val ages = Map("Leo" -> 30, "Jen" -> 25, "Jack" -> 23)

ages("Leo") = 31

```

### 创建一个可变的Map

```
val ages = scala.collection.mutable.Map("Leo" -> 30, "Jen" -> 25, "Jack" -> 23)

ages("Leo") = 31

```

### 使用另外一种方式定义Map元素

	val ages = Map(("Leo", 30), ("Jen", 25), ("Jack", 23))

###创建一个空的HashMap

	val ages = new scala.collection.mutable.HashMap[String, Int]

###访问Map的元素

###获取指定key对应的value，如果key不存在，会报错

```
val leoAge = ages("Leo")

val leoAge = ages("leo")

```

// 使用contains函数检查key是否存在

```
val leoAge = if (ages.contains("leo")) ages("leo") else 0
// getOrElse函数
val leoAge = ages.getOrElse("leo", 0)

```
###修改Map的元素

```
// 更新Map的元素
ages("Leo") = 31

// 增加多个元素
ages += ("Mike" -> 35, "Tom" -> 40)

// 移除元素
ages -= "Mike"

// 更新不可变的map
val ages2 = ages + ("Mike" -> 36, "Tom" -> 40)

// 移除不可变map的元素
val ages3 = ages - "Tom"
```

###遍历Map

```
// 遍历map的entrySet
for ((key, value) <- ages) println(key + " " + value)

// 遍历map的key
for (key <- ages.keySet) println(key)

// 遍历map的value
for (value <- ages.values) println(value)

// 生成新map，反转key和value
for ((key, value) <- ages) yield (value, key)

```

###SortedMap和LinkedHashMap

```
// SortedMap可以自动对Map的key的排序

val ages = scala.collection.immutable.SortedMap("leo" -> 30, "alice" -> 15, "jen" -> 25)

// LinkedHashMap可以记住插入entry的顺序

val ages = new scala.collection.mutable.LinkedHashMap[String, Int]

ages("leo") = 30

ages("alice") = 15

ages("jen") = 25

```

###Map的元素类型—Tuple

```
// 简单Tuple
val t = ("leo", 30)

// 访问Tuple
t._1

// zip操作
val names = Array("leo", "jack", "mike")

val ages = Array(30, 24, 26)

val nameAges = names.zip(ages)

for ((name, age) <- nameAges) println(name + ": " + age)

```

<!--
create time: 2018-03-08 19:56:25
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

