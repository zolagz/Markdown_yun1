```
mysql> select * from dept;
+-----+-----------+
| did | dname     |
+-----+-----------+
|   1 | 市场部    |
|   3 | 人事部    |
|   4 | 教研部    |
+-----+-----------+
3 rows in set (0.00 sec)

mysql> select * from employee;
+-----+--------+--------+------------+------+------+
| eid | ename  | salary | birthday   | sex  | dno  |
+-----+--------+--------+------------+------+------+
|   1 | 张三   |   8000 | 1988-09-01 | 男   |    3 |
|   2 | 李四   |   9000 | 1988-09-01 | 男   |    1 |
|   3 | 王五   |   6000 | 1988-09-01 | 男   |    2 |
|   4 | 赵六   |  10000 | 1988-09-01 | 男   |    3 |
|   5 | 孙七   |  10000 | 1988-09-01 | 男   |    1 |
|   6 | 田八   |  10000 | 1988-09-01 | 男   | NULL |
+-----+--------+--------+------------+------+------+
6 rows in set (0.00 sec)

mysql> delete from employee where eid = 6;
Query OK, 1 row affected (0.00 sec)

mysql> alter table employee add foreign key (dno) references dept(did);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`db1`.`#sql-5d_33`, CONSTRAINT `#sql-5d_33_ibfk_1` FOREIGN KEY (`dno`) REFERENCES `dept` (`did`))

```

以上添加外键失败原因是：

1 ：主表 employee 包含一个null 字段；

2：主表中dno字段中 2 在从表中找不到

解决方法是：


主表中添加该条数据或者将此表该条主表没有的数据改成主表已有的数据即可