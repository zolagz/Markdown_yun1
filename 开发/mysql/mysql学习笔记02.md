###SQL对数据库表的记录进行操作（重点）*	语法：*	向表中插入某些列：insert into 表名 (列名1,列名2,列名3…) values (值1,值2,值3…)*	向表中插入所有列：insert into 表名 values (值1,值2,值3…);*	注意事项*	1.值的类型与数据库中表列的类型一致。*	2.值的顺序与数据库中表列的顺序一致。*	3.值的最大长度不能超过列设置最大长度。*	4.值的类型是字符串或者是日期类型，使用单引号引起来。*	添加记录*	添加某几列>insert into user (id,username,password) values (null,'aaa','123');```	
mysql> insert into user (id,username,password) values (null,'aaa','123');
Query OK, 1 row affected (0.00 sec)
```
*	添加所有列>insert into user values (null,'bbb','123',23,'1993-09-01');```
mysql> insert into user values (null,'bbb','123',23,'1993-09-01');
Query OK, 1 row affected (0.01 sec)

```

* 添加中文记录

>insert into user values (null,'张三','123',23,'1993-09-01');>

```
mysql> insert into user values (null,'张三','123',23,'1993-09-01');
ERROR 1366 (HY000): Incorrect string value: '\xE5\xBC\xA0\xE4\xB8\x89' for column 'username' at row 1```
>直接向数据库中插入中文记录会出现错误！！！
>>解决方法：
>>show variables like '%character%';  --查看数据库中与字符集相关参数：>
>需要将MySQL数据库服务器中的客户端部分的字符集改为gbk。>找到MySQL的安装路径：my.ini文件，修改文件中[client]下的字符集（windows下）
>

[client]

port=3306

[mysql]

default-character-set=**gbk**

* 重新启动MySQL的服务器	services.msc
	

###	SQL修改表的记录*	语法:*	update 表名 set 列名=值,列名=值 [where 条件];*	注意事项*	1.值的类型与列的类型一致。*	2.值的最大长度不能超过列设置的最大长度。*	3.字符串类型和日期类型添加单引号。*	修改某一列的所有值>update user set password = 'abc';```
mysql> update user set password = 'abc';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from user;
+----+----------+----------+------+------------+
| id | username | password | age  | birthday   |
+----+----------+----------+------+------------+
|  1 | bbb      | abc      |   23 | 1993-09-01 |
+----+----------+----------+------+------------+
1 row in set (0.00 sec)
```
*	按条件修改数据
>update user set password = 'xyz' where username = 'bbb';```
mysql> select * from user;
+----+----------+----------+------+------------+
| id | username | password | age  | birthday   |
+----+----------+----------+------+------------+
|  1 | bbb      | xyz      |   23 | 1993-09-01 |
+----+----------+----------+------+------------+
1 row in set (0.00 sec)
```

*	按条件修改多个列

>update user set password='123',age=34 where username='aaa';

```
mysql> update user set password='123',age=34 where username='aaa';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> select * from user;
+----+-----------+----------+------+------------+
| id | username  | password | age  | birthday   |
+----+-----------+----------+------+------------+
|  1 | bbb       | xyz      |   23 | 1993-09-01 |
|  2 | aaa       | 123      |   34 | NULL       |
|  3 | zhang san | 123      |   23 | 1993-09-01 |
+----+-----------+----------+------+------------+
3 rows in set (0.00 sec)

```SQL删除表的记录*	语法：*	delete from 表名 [where 条件];*	注意事项*	1.删除表的记录，指的是删除表中的一行记录。*	2.删除如果没有条件，默认是删除表中的所有记录。*	删除某一条记录
>delete from user where id = 2;```

mysql> delete from user where id = 2;
Query OK, 1 row affected (0.01 sec)

mysql> select * from user;
+----+-----------+----------+------+------------+
| id | username  | password | age  | birthday   |
+----+-----------+----------+------+------------+
|  1 | bbb       | xyz      |   23 | 1993-09-01 |
|  3 | zhang san | 123      |   23 | 1993-09-01 |
+----+-----------+----------+------+------------+
2 rows in set (0.00 sec)

```

* 删除表中的所有记录
>delete from user;

```
mysql> delete from user;
Query OK, 2 rows affected (0.00 sec)

mysql> select * from user;
Empty set (0.00 sec)
```
*	删除表中的记录有两种做法:*	delete from user;*	删除所有记录，属于DML语句，一条记录一条记录删除。事务可以作用在DML语句上的*	truncate table user;*	删除所有记录，属于DDL语句，将表删除，然后重新创建一个结构一样的表。事务不能控制DDL的###SQL查看表的记录（重点）

* 语法:select [distinct] *|列名 from 表 [条件];* 环境的准备```
create table exam(	id int primary key auto_increment,	name varchar(20),	english int,	chinese int,	math	int);insert into exam values (null,'zhang san',85,74,91);insert into exam values (null,'li si',95,90,83);insert into exam values (null,'wang wu',85,84,59);insert into exam values (null,'zhao liu',75,79,76);insert into exam values (null,'tian qi',69,63,98);insert into exam values (null,'li lao ba',89,90,83);
```*	查询所有学生考试成绩信息>select * from exam;*	查询所有学生的姓名和英语成绩>select name,english from exam;* 	查询英语成绩信息（不显示重复的值）>elect distinct english from exam;

* 	查看学生姓名和学生的总成绩>select name,english+chinese+math from exam;*	别名查询
>select name,english+chinese+math as sum from exam;


###	条件查询*	使用where子句*	> , < , >= , <= , <> ,=*	like:模糊查询*	in:范围查询*	条件关联:and , or ,not*	查询李四学生的成绩：>select * from exam where name = '李四';*	查询名称叫李四学生并且英文大于90分select * from exam where name = '李四' and english > 90;*	查询姓李的学生的信息like可以进行模糊查询,在like子句中可以使用_或者%作为占位符。_只能代表一个字符，而%可以代表任意个字符。	* like ‘李_’		：名字中必须是两个字，而且是姓李的。	* like ‘李%’		：名字中姓李的学生，李子后可以是1个或任意个字符。	* like ‘%四’		：名字中以四结尾的。	* like ‘%王%’	：只要名称中包含这个字就可以。>select * from exam where name like '李%';*	查询英语成绩是69,75,89学生的信息>select * from exam where english in (69,75,89);*	排序查询*	使用order by 字段名称 asc/desc;*	查询学生信息，并且按照语文成绩进行排序：>select * from exam order by chinese;	*	查询学生信息，并且按照语文成绩倒序排序：>select * from exam order by chinese desc;*	查询学生信息，先按照语文成绩进行倒序排序，如果成绩相同再按照英语成绩升序排序>select * from exam order by chinese desc,english asc;	*	查询姓李的学生的信息，按照英语成绩降序排序>select * from exam where name like '李%' order by english desc;*	分组统计查询*	聚合函数使用*	sum();*	获取所有学生的英语成绩的总和:>select sum(english) from exam;*	获取所有学生的英语成绩和数学成绩总和：>select sum(english),sum(math) from exam;*	查询姓李的学生的英语成绩的总和>select sum(english) from exam where name like '李%';*	查询所有学生各科的总成绩：>select sum(english)+sum(chinese)+sum(math) from exam;>select sum(english+chinese+math) from exam;与上面的语句有什么不同？* 上面的语句是按照列的方式统计，英语成绩总和+语文成绩总和+数学成绩总和。* 下面的语句先计算英语+数学+语文然后再求和。	* 使用ifnull的函数*	count();*	获得学生的总数> select count(*) from exam;*	获得姓李的学生的个数
>select count(*) from exam where name like '李%';*	max();*	获得数学成绩的最高分：>select max(math) from exam;*	min();*	获得语文成绩的最小值>select min(chinese) from exam;*	avg();*	获取语文成绩的平均值>select avg(chinese) from exam;*	分组查询*	语法：使用group by 字段名称;*	环境准备```
create table orderitem(	id int primary key auto_increment,	product varchar(20),	price double);insert into orderitem values (null,'电视机',2999);insert into orderitem values (null,'电视机',2999);insert into orderitem values (null,'洗衣机',1000);insert into orderitem values (null,'洗衣机',1000);insert into orderitem values (null,'洗衣机',1000);insert into orderitem values (null,'冰箱',3999);insert into orderitem values (null,'冰箱',3999);insert into orderitem values (null,'空调',1999);```
*	按商品名称统计，每类商品所购买的个数：>select product,count(*) from orderitem group by product;*	按商品名称统计，每类商品所花费的总金额：>select product,sum(price) from orderitem group by product;*	按商品名称统计，统计每类商品花费的总金额在5000元以上的商品
* **where的子句后面不能跟着聚合函数。**

*如果现在使用带有聚合函数的条件过滤（分组后条件过滤）需要使用一个关键字having>select product,sum(price) from orderitem  group by product having sum(price) > 5000;*	按商品名称统计，统计每类商品花费的总金额在5000元以上的商品，并且按照总金额升序排序>select product,sum(price) from orderitem  group by product having sum(price) > 5000 order by sum(price) asc;*	总结>	S(select)… F(from)…W(where)…G(group by)…H(having)…O(order by);