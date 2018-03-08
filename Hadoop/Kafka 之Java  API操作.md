# Kafka 之Java  API操作

####创建maven工程，添加依赖

```
<dependency>    <groupId>org.apache.kafka</groupId>    <artifactId>kafka-clients</artifactId>    <version>0.11.0.1</version></dependency>
```

1）	编写代码-写生产者的代码```
/** * 订单的生产者代码 */public class OrderProducer {    public static void main(String[] args) throws InterruptedException {        /* 1、连接集群，通过配置文件的方式         * 2、发送数据-topic:order，value         */        Properties props = new Properties();        props.put("bootstrap.servers", "node01:9092");        props.put("acks", "all");        props.put("retries", 0);        props.put("batch.size", 16384);        props.put("linger.ms", 1);        props.put("buffer.memory", 33554432);        props.put("key.serializer",                "org.apache.kafka.common.serialization.StringSerializer");        props.put("value.serializer",                "org.apache.kafka.common.serialization.StringSerializer");        KafkaProducer<String, String> kafkaProducer = new KafkaProducer<String, String>(props);        for (int i = 0; i < 1000; i++) {            // 发送数据 ,需要一个producerRecord对象,最少参数 String topic, V value            kafkaProducer.send(new ProducerRecord<String, String>("order", "订单信息！"+i));            Thread.sleep(100);        }    }}

```

2）	编写代码-写消费者的代码```
/** * 消费订单数据--- javaben.tojson */public class OrderConsumer {    public static void main(String[] args) {        // 1\连接集群        Properties props = new Properties();        props.put("bootstrap.servers", "node01:9092");        props.put("group.id", "test");        props.put("enable.auto.commit", "true");        props.put("auto.commit.interval.ms", "1000");        props.put("key.deserializer",                "org.apache.kafka.common.serialization.StringDeserializer");        props.put("value.deserializer",                "org.apache.kafka.common.serialization.StringDeserializer");        KafkaConsumer<String, String> kafkaConsumer = new KafkaConsumer<String, String>(props);//        2、发送数据 发送数据需要，订阅下要消费的topic。  order        kafkaConsumer.subscribe(Arrays.asList("order"));        while (true) {            ConsumerRecords<String, String> consumerRecords = kafkaConsumer.poll(100);// jdk queue offer插入、poll获取元素。 blockingqueue put插入原生，take获取元素            for (ConsumerRecord<String, String> record : consumerRecords) {                System.out.println("消费的数据为：" + record.value());            }        }    }}
```<!--
create time: 2018-03-08 11:13:33
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

