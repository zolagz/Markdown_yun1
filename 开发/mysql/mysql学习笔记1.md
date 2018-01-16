
**创建数据库：**



* create database 数据库名称 [character set 字符集 collate 字符集校对规则];```
	mysql> create database db1 character set gbk;
	Query OK, 1 row affected (0.00 sec)

```1.1	查看数据库

* show databases;

```	
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| db1                |
| guan               |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
6 rows in set (0.00 sec)
```

1.2查看某个数据库的定义信息： 

* show create database 数据库名称;

```

mysql> show create database db1;
+----------+-------------------------------------------------------------+
| Database | Create Database                                             |
+----------+-------------------------------------------------------------+
| db1      | CREATE DATABASE `db1` /*!40100 DEFAULT CHARACTER SET gbk */ |
+----------+-------------------------------------------------------------+
1 row in set (0.00 sec)

```1.3 修改数据库

* alter database 数据库名称 character set 字符集 collate 校对规则;```
mysql> alter database db1 character set utf8;
Query OK, 1 row affected (0.00 sec)

mysql> show create database db1;
+----------+--------------------------------------------------------------+
| Database | Create Database                                              |
+----------+--------------------------------------------------------------+
| db1      | CREATE DATABASE `db1` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+--------------------------------------------------------------+
1 row in set (0.00 sec)


```

1.4 	删除数据库

* drop database 数据库名称；

```
mysql> drop database db1;
Query OK, 0 rows affected (0.01 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| guan               |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

```

1.5 切换数据库

* use 数据库名称

```
mysql> use db2;
Database changed

```

* 查看当前正在使用的数据库

```
mysql> select database();
+------------+
| database() |
+------------+
| db2        |
+------------+
1 row in set (0.00 sec)

```

### sql 对数据库表进行操作

2.1 sql 创建表

* 	create table 表名称(字段名称 字段类型(长度) 约束,字段名称 字段类型(长度) 约束…);


java中的类型|mysql中的类型|
-------------------|---------------------------|
byte/short/int/long|tinyint/smallint/int/bigint|
float              |                      float|
double             |                     double|
boolean            |                        bit|
char/String        |               char/varchar|
date               |date/time/datetime/timestamp|

* char和varchar的区别

>char代表是固定长度的字符或字符串。>定义类型char(8),向这个字段存入字符串hello，那么数据库使用三个空格将其补全。> varchar代表的是可变长度的字符串。										
>定义类型varchar(8), 向这个字段存入字符串hello,那么存入到数据库的就是hello。

* datetime和timestamp区别
>datetime就是既有日期又有时间的日期类型，如果没有向这个字段中存值，数据库使用null存入到数据库中
>> timestamp也是既有日期又有时间的日期类型，如果没有向这个字段中存值，数据库使用当前的系统时间存入到数据库中。

__约束__

*	约束作用：保证数据的完整性*	单表约束分类：*	主键约束：primary key 主键约束默认就是唯一 非空的*	唯一约束：unique*	非空约束：not null

__建表语句__

```
create database web_test1;use web_test1;create table user(	id int primary key auto_increment,	username varchar(20) unique,	password varchar(20) not null,	age int,	birthday date);

```

__SQL查看表__

*	查看某个数据库下的所有的表*	语法：show tables;

```
mysql> show tables;
+---------------------+
| Tables_in_web_test1 |
+---------------------+
| user                |
+---------------------+
1 row in set (0.01 sec)
```

*	查看某个表的结构信息*	语法：desc 表名;```
mysql> desc user;
+----------+-------------+------+-----+---------+----------------+
| Field    | Type        | Null | Key | Default | Extra          |
+----------+-------------+------+-----+---------+----------------+
| id       | int(11)     | NO   | PRI | NULL    | auto_increment |
| username | varchar(20) | YES  | UNI | NULL    |                |
| password | varchar(20) | NO   |     | NULL    |                |
| age      | int(11)     | YES  |     | NULL    |                |
| birthday | date        | YES  |     | NULL    |                |
+----------+-------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)

```
__SQL删除表__

*	删除表*	语法：drop table 表名;

```
mysql> drop table user;
Query OK, 0 rows affected (0.01 sec)
```

__SQL修改表__

*	修改表：添加列*	alter table 表名 add 列名 类型(长度) 约束;

```
mysql> alter table user add image varchar(100);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc user;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | int(11)      | NO   | PRI | NULL    | auto_increment |
| username | varchar(20)  | YES  | UNI | NULL    |                |
| password | varchar(20)  | NO   |     | NULL    |                |
| age      | int(11)      | YES  |     | NULL    |                |
| birthday | date         | YES  |     | NULL    |                |
| image    | varchar(100) | YES  |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
6 rows in set (0.00 sec)


```*	修改表：修改列类型，长度和约束*	alter table 表名 modify 列名 类型(长度) 约束;

```
mysql> alter table user modify image varchar(150);
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc  user;
+----------+--------------+------+-----+---------+----------------+
| Field    | Type         | Null | Key | Default | Extra          |
+----------+--------------+------+-----+---------+----------------+
| id       | int(11)      | NO   | PRI | NULL    | auto_increment |
| username | varchar(20)  | YES  | UNI | NULL    |                |
| password | varchar(20)  | NO   |     | NULL    |                |
| age      | int(11)      | YES  |     | NULL    |                |
| birthday | date         | YES  |     | NULL    |                |
| image    | varchar(150) | YES  |     | NULL    |                |
+----------+--------------+------+-----+---------+----------------+
6 rows in set (0.00 sec)
```
*	修改表：删除列*	alter table 表名 drop 列名;

```
mysql> alter table user drop age;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

```*	修改表：修改列名称*	alter table 表名 change 旧列名 新列名 类型(长度) 约束;```
mysql> alter table user change image pic varchar(150);
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

```

*	修改表：修改表名*	rename table 表名 to 新的表名;```
mysql> rename table user to text01;
Query OK, 0 rows affected (0.00 sec)
```
*	修改表：修改表的字符集*	alter table 表名 character set 字符集;

```
mysql> alter table text01 character set gbk;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

```


