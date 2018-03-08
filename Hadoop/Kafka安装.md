###Kafka安装配置

####初始化环境

* 安装JDK ,安装zookeeper，并启动zookeeper
* 创建安装用户


下载安装包并解压

```
tar -zxvf kafka_2.11-1.0.0.tgz -C /export/servers/cd /export/servers/mv kafka_2.11-1.0.0 kafka
```

进入conf中修改配置文件
进入配置目录，查看server.properties文件

```cat server.properties |grep -v "#"
```

![](http://p2ehgqigv.bkt.clouddn.com/18-3-8/62630198.jpg)

通过以上命令，查看到了默认的配置文件，对默认的文件进行修改。修改三个地方1）	Borker.id2）	数据存放的目录，注意目录如果不存在，需要新建下。3）	zookeeper的地址信息```
# broker.id 标识了kafka集群中一个唯一broker。broker.id=0num.network.threads=3num.io.threads=8socket.send.buffer.bytes=102400socket.receive.buffer.bytes=102400socket.request.max.bytes=104857600# 存放生产者生产的数据 数据一般以topic的方式存放# 创建一个数据存放目录 /export/data/kafka  ---  mkdir -p /export/data/kafkalog.dirs=/export/data/kafkanum.partitions=1num.recovery.threads.per.data.dir=1offsets.topic.replication.factor=1transaction.state.log.replication.factor=1transaction.state.log.min.isr=1log.retention.hours=168log.segment.bytes=1073741824log.retention.check.interval.ms=300000# zk的信息zookeeper.connect=zk01:2181,zk02:2181,zk03:2181zookeeper.connection.timeout.ms=6000group.initial.rebalance.delay.ms=0

```

分发配置文件及修改brokerid

分发安装包

```
scp -r /export/servers/kafka/ node02:/export/servers/scp -r /export/servers/kafka/ node03:/export/servers/
```

修改node02，nomde3上的broker.id```
vi /export/servers/kafka/config/server.properties
```
![](http://p2ehgqigv.bkt.clouddn.com/18-3-8/57585359.jpg)


启动集群

```
cd /export/servers/kafka/bin./kafka-server-start.sh /export/servers/kafka/config/server.properties
```