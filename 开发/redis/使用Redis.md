# 使用Redis

###Redis 五种数据类型
* 字符串 （String）
* 哈希 （hash）
* 字符串列表 （list）
* 字符串集合 (set)
* 有序字符串集合(sorted set)

####赋值
 * set key value :设置key持有指定的字符串value 如果该key存在则覆盖稿子。总是返回“OK”
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/6112132.jpg)

###取值
* get key：获取key的value。如果与该key关联的value不是String类型，redis将返回错误信息，因为get命令只能用于获取String value；如果该key不存在，返回(nil)。
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/44390420.jpg)

* getset key value：先获取该key的值，然后在设置该key的值。

![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/93562438.jpg)

####删除

* del key ：删除指定key
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/59346760.jpg)

####数值增减
* incr key：将指定的key的value原子性的递增1.如果该key不存在，其初始值为0，在incr之后其值为1。如果value的值不能转成整型，如hello，该操作将执行失败并返回相应的错误信息。

![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/60859987.jpg)
 
* decr key：将指定的key的value原子性的递减1.如果该key不存在，其初始值为0，在incr之后其值为-1。如果value的值不能转成整型，如hello，该操作将执行失败并返回相应的错误信息。![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/3220096.jpg)

####扩展命令
* incrby key increment：将指定的key的value原子性增加increment，如果该key不存在，器初始值为0，在incrby之后，该值为increment。如果该值不能转成整型，如hello则失败并返回错误信息
 ![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/39108267.jpg)

* 	decrby key decrement：将指定的key的value原子性减少decrement，如果该key不存在，器初始值为0，在decrby之后，该值为decrement。如果该值不能转成整型，如hello则失败并返回错误信息
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/34541785.jpg)

* 	append key value：拼凑字符串。如果该key存在，则在原有的value后追加该值；如果该key不存在，则重新创建一个key/value
  ![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/1806124.jpg)
  
##Hash数据类型

###概述
Redis中的Hash类型可以看成具有String Key和String Value的map容器。所以该类型非常适合于存储值对象的信息。如Username、Password和Age等。如果Hash中包含很少的字段，那么该类型的数据也将仅占用很少的磁盘空间。每一个Hash可以存储4294967295个键值对。####存值

* hset	key field value
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/58123507.jpg)

* hmset key field value field value

![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/78431759.jpg)

####取值

* hget key fileds:返回指定的key中field的值
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/50815200.jpg)

* hmget key fileds:获取key中多个filed的值
 ![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/13915914.jpg)

*	hgetall key：获取key中的所有filed-vaule![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/5488252.jpg)

####删除
* hdel key field [field … ] ：可以删除一个或多个字段，返回值是被删除的字段个数
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/60587736.jpg)

* del key ：删除整个内容
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/94178601.jpg)

####更新内容

![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/98186367.jpg)


####增加数字

* hincrby key field value：为某个key的某个属性增加值


*	hexists key field：判断指定的key中的filed是否存在

*	hlen key：获取key所包含的field的数量

* hkeys key ：获得所有的key* hvals key：获得所有的value


<!--
create time: 2018-03-14 16:24:57
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

