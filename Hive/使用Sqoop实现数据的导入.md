# 使用Sqoop实现数据的导入

视频教程[慕课网](https://www.imooc.com/video/7966)

“导入工具”导入单个表从 RDBMS 到 HDFS。表中的每一行被视为 HDFS 的记录。所有记 录都存储为文本文件的文本数据(或者 Avro、sequence 文件等二进制数据)。下面的语法用于将数据导入 HDFS。	$ sqoop import (generic-args) (import-args)
	

###使用Sqoop导入MySql数据到HDFS中

```
bin/sqoop import --connect jdbc:mysql://node1:3306/userdb \
--username root \
--password admin \
--target-dir /sqoopemp \
--table emp --m 1

```

例2

```
./sqoop import --connect jdbc:mysql://node1:3306/userdb --username root --password admin --table emp --columns 'empno,ename,job,sal,depton' -m 1 --target-dir 'sqoop/emp'

```
[慕课网教程](https://www.imooc.com/video/7966) 2分35秒
-
-columns :指定要导入的字段内容


其中--target-dir 可以用来指定导出数据存放至 HDFS 的目录;

mysql jdbc url 请使用 ip 地址。


为了验证在 HDFS 导入的数据，请使用以下命令查看导入的数据:

 hdfs dfs -cat /sqoopresult/part-m-00000可以看出它会用逗号,分隔 emp 表的数据和字段。


###使用Sqoop导入MySql数据到Hive中

```
bin/sqoop import --hive-import --connect jdbc:mysql://node1:3306/userdb --table emp --username root --password admin --table emp -m 1 
```

执行后，在hive中查看到的结果：

```
hive> show tables;
OK
emp
Time taken: 0.957 seconds, Fetched: 1 row(s)
hive> select * from emp;
OK
1201    gopal   manager 50000   TP
1202    manisha Proof reader    50000   TP
1203    khalil  php dev 30000   AC
1204    prasanth        php dev 30000   AC
1205    kranthi admin   20000   TP
Time taken: 0.515 seconds, Fetched: 5 row(s)
hive> 

```

```
bin/sqoop create-hive-table \--connect jdbc:mysql://node1:3306/userdb \ --table emp \--username root \--password admin \--hive-table test.emp_add_sp

其中:--table emp_add 为 mysql 中的数据库 sqoopdb 中的表。 
--hive-table emp_add_sp 为 hive 中新建的表名称。

```
###使用Sqoop导入MySql数据到Hive中，并指定表名；

 --hive-table 指定表名
```
bin/sqoop import --hive-import --connect jdbc:mysql://node1:3306/userdb --table emp --username root --password admin --table emp -m 1  --hive-table emp1
```

###使用Sqoop导入MySql数据到Hive中，并使用where条件

```
bin/sqoop import --hive-import --connect jdbc:mysql://node1:3306/userdb --table emp --username root --password admin --table emp -m 1  --hive-table emp3 --where 'dept=AC'

```


###使用Sqoop导入MySql数据到Hive中，并使用查询语句

```
bin/sqoop import --hive-import --connect jdbc:mysql://node1:3306/userdb --table emp --username root --password admin --table emp -m 1 --query 'SELECT * FROM salary<3000' --target-dir '/sqoop/emp5'

```


###使用Sqoop导入Hive数据到MySql中


<!--
create time: 2018-03-06 20:35:39
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

