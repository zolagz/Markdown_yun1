# Hive交互的方式


Hive几种使用方式：
	
	1.Hive交互shell      bin/hive
	
	2.Hive JDBC服务(参考java jdbc连接mysql)
	
	3.hive启动为一个服务器，来对外提供服务
		bin/hiveserver2
		nohup bin/hiveserver2 1>/var/log/hiveserver.log 2>/var/log/hiveserver.err &
		
		启动成功后，可以在别的节点上用beeline去连接
		bin/beeline -u jdbc:hive2://mini1:10000 -n root
		
		或者
		bin/beeline
		! connect jdbc:hive2://mini1:10000
	
	4.Hive命令 
		hive  -e  ‘sql’
		bin/hive -e 'select * from t_test'

这里着重介绍一下第三种方式

通过scp命令把安装好的hive复制一份到node2上

```
scp -r apache-hive-1.2.1-bin/ root@node2:/usr/local/dev/

```

在node2上，进入hive/bin目录下

输入# beeline

beeline> !connect jdbc:hive2://node1:10000

```
[root@node2 apache-hive-1.2.1-bin]# cd bin/
[root@node2 bin]# beeline 
Beeline version 1.2.1 by Apache Hive
beeline> !connect jdbc:hive2://node1:10000
Connecting to jdbc:hive2://node1:10000
Enter username for jdbc:hive2://node1:10000: root
Enter password for jdbc:hive2://node1:10000: ******
Connected to: Apache Hive (version 1.2.1)
Driver: Hive JDBC (version 1.2.1)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1:10000> 

```

创建数据表，并指定分割符为逗号“，”

```
>use itcast;

>create table t_t3 (id int ,name string) row format delimited fields terminated by ',';

```

在node1上，新建一个txt文档

```
vi a.txt

1,allen
2,jim
3,tom
4,jasper

```

上传到hadoop上

```
 hadoop fs -put a.txt /user/hive/warehouse/itcast.db/t_t3
```
然后在node2上的hive程序中执行：

```
0: jdbc:hive2://node1:10000> use itcast;
No rows affected (1.447 seconds)
0: jdbc:hive2://node1:10000> select * from t_t3;
+----------+------------+--+
| t_t3.id  | t_t3.name  |
+----------+------------+--+
| 1        | allen      |
| 2        | jim        |
| 3        | tom        |
| 4        | jasper     |
| NULL     | NULL       |
+----------+------------+--+
5 rows selected (1.453 seconds)
0: jdbc:hive2://node1:10000> 

```


根据数据创建数据库表2

```
vi b.txt

zhangsan	beijing,shanghai,tianjin,hangzhou


wangwu		shanghai,chengdu,wuhan,haerbin


 hadoop fs -put c.txt /user/hive/warehouse/itcast.db/t_t2

```
建表语句


```
create table t_t2(name string,work_locations array<string>) ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t' COLLECTION ITEMS TERMINATED BY ',';

```

###测试数据3

```
vi c.txt

1,zhangsan,唱歌:非常喜欢-跳舞:喜欢-游泳:一般般

2,lisi,打游戏:非常喜欢-篮球:不喜欢


 hadoop fs -put c.txt /user/hive/warehouse/itcast.db/t_t1
```

建表语句

```
create table t_t1(id int,name string,hobby map<string,string>) row format delimited fields terminated by ',' collection items terminated by '-' map keys terminated by ':' ;
```


<!--
create time: 2018-02-26 15:49:44
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

