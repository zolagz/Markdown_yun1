# Kafka命令行操作

* 在node1上创建一个订单的topic

```
bin/kafka-topics.sh --create --zookeeper node1:2181 --replication-factor 1 --partitions 1 --topic order

```
![](http://p2ehgqigv.bkt.clouddn.com/18-3-8/74154398.jpg)

* 在node2上	编写代码启动一个生产者，生产数据

```
bin/kafka-console-producer.sh --broker-list node1:9092 --topic order

```
![](http://p2ehgqigv.bkt.clouddn.com/18-3-8/12965176.jpg)

* 在node3上  编写代码启动给一个消费者，消费数据

```
bin/kafka-console-consumer.sh --zookeeper node1:2181 --from-beginning --topic order

```
![](http://p2ehgqigv.bkt.clouddn.com/18-3-8/13603096.jpg)


<!--
create time: 2018-03-08 12:19:28
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

