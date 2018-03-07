# Hive数据导入1

##Load

在将数据加载到表中时，Hive 不会进行任何转换。加载操作是将数据文件移动到与 Hive表对应的位置的纯复制/移动操作。 

####语法结构
```LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)]
```###说明:####1、 filepath 
	
*	相对路径，例如:project/data1
*	绝对路径，例如:/user/hive/project/data1*	完整 URI，例如:hdfs://namenode:9000/user/hive/project/data1	
*	filepath 可以引用一个文件(在这种情况下，Hive 将文件移动到表中)，或	者它可以是一个目录(在这种情况下，Hive 将把该目录中的所有文件移动到表中)。 

####2、 LOCAL
如果指定了 LOCAL， load 命令将在本地文件系统中查找文件路径。	
load 命令会将 filepath 中的文件复制到目标文件系统中。目标文件系统由表 的位置属性决定。被复制的数据文件移动到表的数据对应的位置。	
如果没有指定 LOCAL 关键字，如果 filepath 指向的是一个完整的 URI，hive 会直接使用这个 URI。 否则:如果没有指定 schema 或者 authority，Hive 会使 用在 hadoop 配置文件中定义的 schema 和 authority，fs.default.name 指定了	Namenode 的 URI。 

####3、 OVERWRITE如果使用了 OVERWRITE 关键字，则目标表(或者分区)中的内容会被删除， 然后再将 filepath 指向的文件/目录中的内容添加到表/分区中。
如果目标表(分区)已经有一个文件，并且文件名和 filepath 中的文件名冲突， 那么现有的文件会被新文件所替代。



将student01.txt数据导入到t2

```
load data local inpath '/root/data/student01.txt' into table t2;
```

将/root/data下的所有数据文件导入到t3表中；并且覆盖原来的数据

```
load data local inpath '/root/data/' overwrite into table t3;
```


将HDFS中 /input/student01.txt导入t3

```
load data inpath '/input/student/01.txt' overwrite into table t3;
```

![](http://p2ehgqigv.bkt.clouddn.com/18-3-6/66642425.jpg)

###Insert

Hive 中 insert 主要是结合 select 查询语句使用，将查询结果插入到表中，例如: 



```
insert overwrite table stu_buckselect * from student cluster by(Sno); 

```

需要保证查询结果列的数目和需要插入数据表格的列数目一致. 

如果查询出来的数据类型和插入表格对应的列数据类型不一致，将会进行转换，但是不能保证转换一定成功，转换失败的数据将会为 NULL。

可以将一个表查询出来的数据插入到原表中, 结果相当于自我复制了一份数据

```
from source_table insert overwrite table table1 [partition (partcol=val1,partclo2=val2)] select_statement1 

insert overwrite table tablename2 [partition (partcol1=val,partclo2=val2)]select_statement2..
```

动态分区插入:INSERT OVERWRITE TABLE tablename PARTITION (partcol1[=val1], partcol2[=val2] ...) select_statement FROM from_statement动态分区是通过位置来对应分区值的。原始表 select 出来的值和输出 partition 的值的关系仅仅是通过位置来确定的，和名字并没有关系。

<!--
create time: 2018-03-06 20:07:33
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

