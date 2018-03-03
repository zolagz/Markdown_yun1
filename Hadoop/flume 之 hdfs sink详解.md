# flume 之 hdfs sink详解

```
Flume有HDFS Sink，可以将Source进来的数据写入到hdfs中。

HDFS Sink具体的逻辑代码是在HDFSEventSink这个类中。

HDFS Sink跟写文件相关的配置如下：

hdfs.path -> hdfs目录路径

hdfs.filePrefix -> 文件前缀。默认值FlumeData

hdfs.fileSuffix -> 文件后缀

hdfs.rollInterval -> 多久时间后close hdfs文件。单位是秒，默认30秒。设置为0的话表示不根据时间close hdfs文件

hdfs.rollSize -> 文件大小超过一定值后，close文件。默认值1024，单位是字节。设置为0的话表示不基于文件大小

hdfs.rollCount -> 写入了多少个事件后close文件。默认值是10个。设置为0的话表示不基于事件个数

hdfs.fileType -> 文件格式， 有3种格式可选择：SequenceFile, DataStream or CompressedStream

hdfs.batchSize -> 批次数，HDFS Sink每次从Channel中拿的事件个数。默认值100

hdfs.minBlockReplicas -> HDFS每个块最小的replicas数字，不设置的话会取hadoop中的配置

hdfs.maxOpenFiles -> 允许最多打开的文件数，默认是5000。如果超过了这个值，越早的文件会被关闭

serializer -> HDFS Sink写文件的时候会进行序列化操作。会调用对应的Serializer借口，可以自定义符合需求的Serializer

hdfs.retryInterval -> 关闭HDFS文件失败后重新尝试关闭的延迟数，单位是秒

hdfs.callTimeout -> HDFS操作允许的时间，比如hdfs文件的open，write，flush，close操作。单位是毫秒，默认值是10000

```

[flume 之 hdfs sink详解](http://www.aboutyun.com/forum.php?mod=viewthread&tid=21422&extra=page%3D1)



<!--
create time: 2018-03-02 17:34:31
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

