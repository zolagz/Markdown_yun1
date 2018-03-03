# Apache Sqoop的安装

Sqoop 是 Hadoop 和关系数据库服务器之间传送数据的一种工具。它是用来从关系数据 库如:MySQL，Oracle 到 Hadoop 的 HDFS，并从 Hadoop 的文件系统导出数据到关系数据 库。由 Apache 软件基金会提供。

![](http://p2ehgqigv.bkt.clouddn.com/18-3-1/6993314.jpg)

Sqoop 工作机制是将导入或导出命令翻译成 mapreduce 程序来实现。 在翻译出的 mapreduce 中主要是对 inputformat 和 outputformat 进行定制。###sqoop安装

安装 sqoop 的前提是已经具备 java 和 hadoop 的环境。 最新稳定版:[sqoop-1.4.6](https://pan.baidu.com/s/1i5Vl4fr)####配置文件修改:```
cd $SQOOP_HOME/confmv sqoop-env-template.sh sqoop-env.shvi sqoop-env.sh<!--导入hadoop的安装路径-->export HADOOP_COMMON_HOME=/root/apps/hadoop/ 
<!--导入hadoop的安装路径-->
export HADOOP_MAPRED_HOME=/root/apps/hadoop/ <!--导入Hive的安装路径-->
export HIVE_HOME=/root/apps/hive 
```

加入  [mysql 的 jdbc 驱动包](https://pan.baidu.com/s/1ggA7cXH)上传mysql-connector-java-5.1.28.jar到sqoop/lib/目录



验证启动```
bin/sqoop list-databases --connect jdbc:mysql://localhost:3306/ --username root --password hadoop
```本命令会列出所有 mysql 的数据库。 到这里，整个 Sqoop 安装工作完成。
![](http://p2ehgqigv.bkt.clouddn.com/18-3-1/32468566.jpg)




###导入mysql表数据到HDFS

mysql[测试数据表](https://pan.baidu.com/s/1i69NaYx)
通过Navicate for Mysql客户端连接到node1上的数据库，新建userdb数据库；utf-8格式

导入测试数据表
![](http://p2ehgqigv.bkt.clouddn.com/18-3-1/60600832.jpg)

然后在sqoop目录下执行如下操作：

```
bin/sqoop import \
--connect jdbc:mysql://node1:3306/userdb \
--username root \
--password admin \
--target-dir /sqoopresult \
--table emp --m 1

```
执行完成之后

```
18/02/13 06:10:25 INFO mapreduce.JobSubmitter: number of splits:1
18/02/13 06:10:25 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1517674120978_0003
18/02/13 06:10:26 INFO impl.YarnClientImpl: Submitted application application_1517674120978_0003
18/02/13 06:10:26 INFO mapreduce.Job: The url to track the job: http://node1:8088/proxy/application_1517674120978_0003/
18/02/13 06:10:26 INFO mapreduce.Job: Running job: job_1517674120978_0003
18/02/13 06:10:49 INFO mapreduce.Job: Job job_1517674120978_0003 running in uber mode : false
18/02/13 06:10:49 INFO mapreduce.Job:  map 0% reduce 0%
18/02/13 06:11:01 INFO mapreduce.Job:  map 100% reduce 0%
18/02/13 06:11:01 INFO mapreduce.Job: Job job_1517674120978_0003 completed successfully
18/02/13 06:11:02 INFO mapreduce.Job: Counters: 30
        File System Counters
                FILE: Number of bytes read=0
                FILE: Number of bytes written=138470
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=87
                HDFS: Number of bytes written=151
                HDFS: Number of read operations=4
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
        Job Counters 
                Launched map tasks=1
                Other local map tasks=1
                Total time spent by all maps in occupied slots (ms)=9203
                Total time spent by all reduces in occupied slots (ms)=0
                Total time spent by all map tasks (ms)=9203
                Total vcore-milliseconds taken by all map tasks=9203
                Total megabyte-milliseconds taken by all map tasks=9423872
        Map-Reduce Framework
                Map input records=5
                Map output records=5
                Input split bytes=87
                Spilled Records=0
                Failed Shuffles=0
                Merged Map outputs=0
                GC time elapsed (ms)=146
                CPU time spent (ms)=1530
                Physical memory (bytes) snapshot=104243200
                Virtual memory (bytes) snapshot=2067804160
                Total committed heap usage (bytes)=18026496
        File Input Format Counters 
                Bytes Read=0
        File Output Format Counters 
                Bytes Written=151
18/02/13 06:11:02 INFO mapreduce.ImportJobBase: Transferred 151 bytes in 43.9774 seconds (3.4336 bytes/sec)
18/02/13 06:11:02 INFO mapreduce.ImportJobBase: Retrieved 5 records.
[root@node1 sqoop]#  
```

通过hdf 查看导入的数据；

	hdfs dfs -cat /sqoopresult/part-m-00000

###导入mysql表数据到HIVE

将关系型数据的表结构复制到hive中

```
bin/sqoop create-hive-table \
--connect jdbc:mysql://node1:3306/userdb \
--table emp_add \
--username root \
--password admin \
--hive-table itcast.emp_add_sp

```
其中 --table emp_add为mysql中的数据库sqoopdb中的表   
	 --hive-table emp_add_sp 为hive中新建的表名称
	 特别要注意，--hive-table itcast.emp_add_sp 这里要求必须存在itcast数据库表，不然执行报错说数据库不存在

```
18/02/13 06:54:08 INFO hive.HiveImport: FAILED: SemanticException [Error 10072]: Database does not exist: itcast
```
   
从关系数据库导入文件到hive中

```
bin/sqoop import \
--connect jdbc:mysql://node1:3306/userdb \
--username root \
--password admin \
--table emp_add \
--hive-table itcast.emp_add_sp \
--hive-import \
--m 1
```


-----------------------------------------------------------------
###导入表数据子集

```
bin/sqoop import \
--connect jdbc:mysql://node-21:3306/sqoopdb \
--username root \
--password admin \
--where "city ='sec-bad'" \
--target-dir /wherequery \
--table emp_add --m 1

```

```
bin/sqoop import \
--connect jdbc:mysql://node1:3306/userdb \
--username root \
--password admin \
--target-dir /wherequery12 \
--query 'select id,name,deg from emp WHERE  id>1203 and $CONDITIONS' \
--split-by id \
--fields-terminated-by '\t' \
--m 1
```
-----------------------------------------------------------------
增量导入

```
bin/sqoop import \
--connect jdbc:mysql://node-21:3306/sqoopdb \
--username root \
--password admin \
--table emp --m 1 \
--incremental append \
--check-column id \
--last-value 1205

```
-----------------------------------------------------------------
sqoop 导出数据至mysql

```
emp/emp_data

1201,gopal,manager,50000,TP
1202,manisha,preader,50000,TP
1203,kalil,php dev,30000,AC
1204,prasanth,php dev,30000,AC
1205,kranthi,admin,20000,TP
1206,satishp,grpdes,20000,GR
```

首先需要手动创建mysql中的目标表：

```
mysql> USE sqoopdb;
mysql> CREATE TABLE employee ( 
   id INT NOT NULL PRIMARY KEY, 
   name VARCHAR(20), 
   deg VARCHAR(20),
   salary INT,
   dept VARCHAR(10));

```

然后执行导出命令

```
bin/sqoop export \
--connect jdbc:mysql://node-21:3306/sqoopdb \
--username root \
--password admin \
--table employee \
--export-dir /emp/emp_data
```



----------------------------------------------------------
1)列出mysql数据库中的所有数据库命令

```
bin/sqoop list-databases \
--connect jdbc:mysql://node-21:3306 \
--username root \
--password admin
```   
2)连接mysql并列出数据库中的表命令

```
bin/sqoop list-tables \
--connect jdbc:mysql://node-21:3306/sqoopdb \
--username root \
--password admin
```


<!--
create time: 2018-03-01 09:37:13
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

