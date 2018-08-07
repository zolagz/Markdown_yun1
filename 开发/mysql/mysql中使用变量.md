# mysql中使用变量



mysql set @a:=1; 冒号是什么 50

```
mysql> set @a=4;
Query OK, 0 rows affected (0.00 sec)

mysql> set @b:=4; //注意，这里多了个冒号
Query OK, 0 rows affected (0.00 sec)

```
请问给变量赋值时多一个冒号有什么作用吗？ 

mysql中变量不用事前申明，在用的时候直接用“@变量名”使用就可以了。

第一种用法：set @num=1; 或set @num:=1; //这里要使用变量来保存数据，直接使用@num变量

第二种用法：select @num:=1; 或 select @num:=字段名 from 表名 where ……
注意上面两种赋值符号，使用set时可以用“=”或“：=”，**但是使用select时必须用“：=赋值”**


### 排序学生成绩

```
mysql> select t1.user_id,t1.score,@rownum:=@rownum + 1 as rownum from(select * from sql_rank order by score desc) as t1 ,(select @rownum:=0)r;
+---------+-------+--------+
| user_id | score | rownum |
+---------+-------+--------+
|     107 |    85 |      1 |
|     104 |    80 |      2 |
|     106 |    70 |      3 |
|     103 |    60 |      4 |
|     108 |    60 |      5 |
|     100 |    50 |      6 |
|     105 |    50 |      7 |
|     101 |    30 |      8 |
|     102 |    20 |      9 |
+---------+-------+--------+
9 rows in set (0.01 sec)

```

### 方法2

```
mysql> set @num = 0;
Query OK, 0 rows affected (0.00 sec)
mysql> select t1.user_id,t1.score,@num:=@num + 1 as rownum from (select * from sql_rank order by score desc) as t1;
+---------+-------+--------+
| user_id | score | rownum |
+---------+-------+--------+
|     107 |    85 |      1 |
|     104 |    80 |      2 |
|     106 |    70 |      3 |
|     103 |    60 |      4 |
|     108 |    60 |      5 |
|     100 |    50 |      6 |
|     105 |    50 |      7 |
|     101 |    30 |      8 |
|     102 |    20 |      9 |
+---------+-------+--------+
9 rows in set (0.01 sec)
mysql>

```


### 第三种方法

```
mysql> select user_id,score,(@rowNum:=@rowNum+1) as rowNo from sql_rank,(select (@rowNum :=0) ) b  order by sql_rank.score desc;
+---------+-------+-------+
| user_id | score | rowNo |
+---------+-------+-------+
|     107 |    85 |     1 |
|     104 |    80 |     2 |
|     106 |    70 |     3 |
|     103 |    60 |     4 |
|     108 |    60 |     5 |
|     100 |    50 |     6 |
|     105 |    50 |     7 |
|     101 |    30 |     8 |
|     102 |    20 |     9 |
+---------+-------+-------+
9 rows in set (0.00 sec)
```

### 查出某个学生在所有用户成绩中的排名

```
mysql> select u.user_id ,u.rowNo from (select user_id,score,(@rowNum:=@rowNum+1) as rowNo from sql_rank,(select (@rowNum :=0) ) b  order by sql_rank.score desc) u where u.user_id ="103";
+---------+-------+
| user_id | rowNo |
+---------+-------+
|     103 |     4 |
+---------+-------+
1 row in set (0.00 sec)

mysql>
```

<!--

create time: 2018-06-23 17:59:57
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

