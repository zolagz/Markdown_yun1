# Hadoop集群启动


### 启动方式
要启动 Hadoop 集群，需要启动 HDFS 和 YARN 两个集群。
注意:首次启动 HDFS 时，必须对其进行格式化操作。本质上是一些清理和 准备工作，因为此时的 HDFS 在物理上还是不存在的。
hdfs namenode–format 或者 hadoop namenode –format 

### 单节点逐个启动在主节点上使用以下命令启动 HDFS NameNode: 	hadoop-daemon.sh start namenode

在每个从节点上使用以下命令启动 HDFS DataNode:
	 	hadoop-daemon.sh start datanode在主节点上使用以下命令启动 YARN ResourceManager: 	yarn-daemon.sh start resourcemanager
在每个从节点上使用以下命令启动 YARN nodemanager:	
	 yarn-daemon.sh start nodemanager
以上脚本位于$HADOOP_PREFIX/sbin/目录下。如果想要停止某个节点上某个 角色，只需要把命令中的 start 改为 stop 即可。
### 脚本一键启动
如果配置了 etc/hadoop/slaves 和 ssh 免密登录，则可以使用程序脚本启动 所有 Hadoop 两个集群的相关进程，在主节点所设定的机器上执行。
	hdfs:$HADOOP_PREFIX/sbin/start-dfs.sh 
	yarn: $HADOOP_PREFIX/sbin/start-yarn.sh 

停止集群:
	
	stop-dfs.sh、stop-yarn.sh
	

###集群 web-ui

一旦 Hadoop 集群启动并运行，可以通过 web-ui 进行集群查看，如下所述: 

NameNode http://nn_host:port/ 默认 50070.ResourceManager http://rm_host:port/ 默认 8088.

![](http://p2ehgqigv.bkt.clouddn.com/18-3-6/7426016.jpg)

![](http://p2ehgqigv.bkt.clouddn.com/18-3-6/89684608.jpg)

<!--
create time: 2018-03-06 18:11:40
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

