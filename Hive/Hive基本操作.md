# Hive基本操作

Hive配置单元包含一个名为 default 默认的数据库.


###创建表：
	
	create database [if not exists] <database name>；---创建数据库
	
	show databases;  --显示所有数据库
	
	drop database if exists <database name> [restrict|cascade];  --删除数据库，默认情况下，hive不允许删除含有表的数据库，要先将数据库中的表清空才能drop，否则会报错
	--加入cascade关键字，可以强制删除一个数据库,默认是restrict，表示有限制的
		eg.  hive> drop database if exists users cascade;
	
	use <database name>; --切换数据库
	
	
###例子
创建表 t_a1

```
create table t_a1(id int,name string) row format delimited fields terminated by ',';

```

创建表 t_b

```
create table b(id int,name string) row format delimited fields terminated by ',';

```

从本地导入数据：

```
load data local inpath '/root/hivedata/a.txt' into table t_a;

load data local inpath '/root/hivedata/b.txt' into table t_b;
```
	

###分区表（PARTITIONED BY）

     分区建表分为2种，一种是单分区，也就是说在表文件夹目录下只有一级文件夹目录。另外一种是多分区，表文件夹下出现多文件夹嵌套模式。
	
	 单分区建表语句：create table day_table (id int, content string) partitioned by (dt string);单分区表，按天分区，在表结构中存在id，content，dt三列。

	 双分区建表语句：create table day_hour_table (id int, content string) partitioned by (dt string, hour string);双分区表，按天和小时分区，在表结构中新增加了dt和hour两列。

	 导入数据
	 LOAD DATA local INPATH '/root/hivedata/dat_table.txt' INTO TABLE day_table partition(dt='2017-07-07');
	 
	 LOAD DATA local INPATH '/root/hivedata/dat_table.txt' INTO TABLE day_hour_table PARTITION(dt='2017-07-07', hour='08');
	 
	 基于分区的查询：

	 SELECT day_table.* FROM day_table WHERE day_table.dt = '2017-07-07';

     查看分区：

     show partitions day_hour_table;  

	 总的说来partition就是辅助查询，缩小查询范围，加快数据的检索速度和对数据按照一定的规格和条件进行管理。



###ROW FORMAT DELIMITED（指定分隔符） 
	
	create table day_table (id int, content string) partitioned by (dt string) row format delimited fields terminated by ',';   ---指定分隔符创建分区表

	复杂类型的数据表指定分隔符
	
	create table complex_array(name string,work_locations array<string>) ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t' COLLECTION ITEMS TERMINATED BY ',';
	
	数据如下：
	 zhangsan   beijing,shanghai,tianjin,hangzhou
	 wangwu   shanghai,chengdu,wuhan,haerbin

	create table t_t1(id int,name string,hobby map<string,string>)
	row format delimited 
	fields terminated by ','
	collection items terminated by '-'
	map keys terminated by ':' ;

	数据：
	1,zhangsan,唱歌:非常喜欢-跳舞:喜欢-游泳:一般般
	2,lisi,打游戏:非常喜欢-篮球:不喜欢


<!--
create time: 2018-02-26 19:25:18
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

