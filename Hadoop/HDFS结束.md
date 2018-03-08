
HDFS的来源：

源自于Google的GFS论文；
* 发表于2013年10月
* HDFS是GFS克隆版

Hadoop Distributed File system

* 易于扩展的分布式文件系统
* 运行在大量的分布式文件系统
* 为大量用户提供性能不错的文件存取服务


HDFS架构详解：

Namenode:是一个中心服务器，单一节点（简化系统的设计和实现），负责管理文件系统的名字空间（namespace）已经客户端对文件的访问。

文件操作：NameNode负责文件元数据的操作，DataNode负责粗粒文件内容的读写请求，跟文件内容相关的数据流不经过NameNode，只会询问它跟那么DataNode联系，否则NameNode会成为系统的瓶颈。

副本放在那些DataNode上由NameNode来控制，根据全局情况来做出块放置决定，读取文件夹时NameNode尽量让用户读取最近的副本。降低带块消耗和读取延时。

Namenode全权管理数据块的复制。它周期性的从集群中的每个datanode接收心跳信号和块状态报告。接收到心跳信号意味之该datanode节点工作正常。块状态报告包含了一个DataNode上所有数据块的列表、




