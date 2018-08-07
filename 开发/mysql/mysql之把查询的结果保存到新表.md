###mysql之把查询的结果保存到新表

[参考链接](https://blog.csdn.net/hsd2012/article/details/51434125)

有时我们要把查询的结果保存到新表里，创建新表，查询，插入显得十分麻烦。

	create table 新表名 like 已有表

数据的复制

```
mysql> create table tab1 like emp;
Query OK, 0 rows affected (0.03 sec)

mysql> insert into tab1 select * from emp;
Query OK, 14 rows affected (0.01 sec)
Records: 14  Duplicates: 0  Warnings: 0

mysql> select * from tab1;
+-------+-----------+-----------+------+------------+----------+----------+--------+
| empno | ename     | job       | mgr  | hiredate   | sal      | COMM     | deptno |
+-------+-----------+-----------+------+------------+----------+----------+--------+
|  1001 | 甘宁      | 文员      | 1013 | 2000-12-17 |  8000.00 |     NULL |     20 |
|  1002 | 黛绮丝    | 销售员    | 1006 | 2001-02-20 | 16000.00 |  3000.00 |     30 |
|  1003 | 殷天正    | 销售员    | 1006 | 2001-02-22 | 12500.00 |  5000.00 |     30 |
|  1004 | 刘备      | 经理      | 1009 | 2001-04-02 | 29750.00 |     NULL |     20 |
|  1005 | 谢逊      | 销售员    | 1006 | 2001-09-28 | 12500.00 | 14000.00 |     30 |
|  1006 | 关羽      | 经理      | 1009 | 2001-05-01 | 28500.00 |     NULL |     30 |
|  1007 | 张飞      | 经理      | 1009 | 2001-09-01 | 24500.00 |     NULL |     10 |
|  1008 | 诸葛亮    | 分析师    | 1004 | 2007-04-19 | 30000.00 |     NULL |     20 |
|  1009 | 曾阿牛    | 董事长    | NULL | 2001-11-17 | 50000.00 |     NULL |     10 |
|  1010 | 韦一笑    | 销售员    | 1006 | 2001-09-08 | 15000.00 |     0.00 |     30 |
|  1011 | 周泰      | 文员      | 1008 | 2007-05-23 | 11000.00 |     NULL |     20 |
|  1012 | 程普      | 文员      | 1006 | 2001-12-03 |  9500.00 |     NULL |     30 |
|  1013 | 庞统      | 分析师    | 1004 | 2001-12-03 | 30000.00 |     NULL |     20 |
|  1014 | 黄盖      | 文员      | 1007 | 2002-01-23 | 13000.00 |     NULL |     10 |
+-------+-----------+-----------+------+------------+----------+----------+--------+
14 rows in set (0.00 sec)

```	
###  从查询结果中创建表

使用如下命令，从一个select语句中创建新的数据表。

	create table ... select ...

该语句优点是不仅仅创建了数据表，还复制了数据表中的数据。缺点是该语句不会复制所有的数据 列属性，如索引、auto_increment等。因为结果集本身就不带索引等。 



其实直接可以搞定。例如把表2的查询结果插入表1：
如果表存在：

	insert into tab1 select * from tab2

如果表不存在：

	create table tab1 as select * from tab2

<!--
create time: 2018-06-20 15:22:04
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

