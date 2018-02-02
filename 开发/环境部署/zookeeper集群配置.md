
##下载安装包、解压

安装前需要安装好jdk

```
#tar -zxvf zookeeper-3.4.6.tar.gz

#mv zookeeper-3.4.6 zookeeper
```

##修改环境变量（注意：3台zookeeper都需要修改）

```
vi /etc/profile
export ZOOKEEPER_HOME=/usr/local/dev/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin
source /etc/profile

```

##修改Zookeeper配置文件

```
#cd zookeeper/conf

#cp zoo_sample.cfg zoo.cfg

#vi zoo.cfg
```

修改内容：

```
dataDir=/usr/local/dev/zookeeper/zkdata

server.1=node1:2888:3888     ## (心跳端口、选举端口)
server.2=node2:2888:3888
server.3=node3:2888:3888

```
创建文件夹：

```
#cd /usr/local/dev/zookeeper/

#mkdir zkdata

```
在data文件夹下新建myid文件，myid的文件内容为：

```
#cd zkdata

#touch myid

#echo 1 > myid
```


修改hosts

```
	vi /etc/hosts
	
	127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
	
	::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

	192.168.25.130 node1

	192.168.25.131 node2

	192.168.25.132 node3

```
	
###关闭防火墙 

1） 永久性生效，重启后不会复原

```
开启： chkconfig iptables on

关闭： chkconfig iptables off
```

2） 即时生效，重启后复原

```
开启： service iptables start

关闭： service iptables stop
```

查看防火墙状态：
	
	 service iptables status

###遇到的bug1

```
JMX enabled by default
Using config: /usr/local/dev/zookeeper-3.4.6/bin/../conf/zoo.cfg
Error contacting service. It is probably not running.
[root@node1 bin]# cat zookeeper.out
2018-01-30 05:30:14,862 [myid:] - INFO  [main:QuorumPeerConfig@103] - Reading configuration from: /usr/local/dev/zookeeper-3.4.6/bin/../conf/zoo.cfg
2018-01-30 05:30:14,868 [myid:] - ERROR [main:QuorumPeerMain@85] - Invalid config, exiting abnormally
org.apache.zookeeper.server.quorum.QuorumPeerConfig$ConfigException: Error processing /usr/local/dev/zookeeper-3.4.6/bin/../conf/zoo.cfg
        at org.apache.zookeeper.server.quorum.QuorumPeerConfig.parse(QuorumPeerConfig.java:123)
        at org.apache.zookeeper.server.quorum.QuorumPeerMain.initializeAndRun(QuorumPeerMain.java:101)
        at org.apache.zookeeper.server.quorum.QuorumPeerMain.main(QuorumPeerMain.java:78)
Caused by: java.lang.NumberFormatException: For input string: "3888     ## (å¿è·³ç«¯å£ãéä¸¾ç«¯å£)"
        at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65)
        at java.lang.Integer.parseInt(Integer.java:492)
        at java.lang.Integer.parseInt(Integer.java:527)
        at org.apache.zookeeper.server.quorum.QuorumPeerConfig.parseProperties(QuorumPeerConfig.java:193)
        at org.apache.zookeeper.server.quorum.QuorumPeerConfig.parse(QuorumPeerConfig.java:119)
        ... 2 more
Invalid config, exiting abnormally
[root@node1 bin]# cd ../conf

```
根据提示：修改conf/zoo.cfg 删除 ##注释

###遇到的bug2

```
2018-01-30 05:32:07,064 [myid:1] - INFO  [WorkerReceiver[myid=1]:FastLeaderElection@597] - Notification: 1 (message format version), 1 (n.leader), 0x0 (n.zxid), 0x1 (n.round), LOOKING (n.state), 1 (n.sid), 0x0 (n.peerEpoch) LOOKING (my state)
2018-01-30 05:32:07,067 [myid:1] - WARN  [WorkerSender[myid=1]:QuorumCnxManager@382] - Cannot open channel to 2 at election address node2/192.168.25.131:3888
java.net.NoRouteToHostException: No route to host
        at java.net.PlainSocketImpl.socketConnect(Native Method)
        at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:339)
        at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:200)
        at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:182)
        at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
        at java.net.Socket.connect(Socket.java:579)
        at org.apache.zookeeper.server.quorum.QuorumCnxManager.connectOne(QuorumCnxManager.java:368)
        at org.apache.zookeeper.server.quorum.QuorumCnxManager.toSend(QuorumCnxManager.java:341)
        at org.apache.zookeeper.server.quorum.FastLeaderElection$Messenger$WorkerSender.process(FastLeaderElection.java:449)
        at org.apache.zookeeper.server.quorum.FastLeaderElection$Messenger$WorkerSender.run(FastLeaderElection.java:430)
        at java.lang.Thread.run(Thread.java:745)
2018-01-30 05:32:19,724 [myid:1] - WARN  [QuorumPeer[myid=1]/0:0:0:0:0:0:0:0:2181:QuorumCnxManager@382] - Cannot open channel to 3 at election address node3/192.168.25.132:3888
java.net.NoRouteToHostException: No route to host
        at java.net.PlainSocketImpl.socketConnect(Native Method)
        at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:339)
        at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:200)
        at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:182)
        at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
        at java.net.Socket.connect(Socket.java:579)
        at org.apache.zookeeper.server.quorum.QuorumCnxManager.connectOne(QuorumCnxManager.java:368)
        at org.apache.zookeeper.server.quorum.QuorumCnxManager.connectAll(QuorumCnxManager.java:402)
        at org.apache.zookeeper.server.quorum.FastLeaderElection.lookForLeader(FastLeaderElection.java:840)
        at org.apache.zookeeper.server.quorum.QuorumPeer.run(QuorumPeer.java:762)
2018-01-30 05:32:19,724 [myid:1] - INFO  [QuorumPeer[myid=1]/0:0:0:0:0:0:0:0:2181:FastLeaderElection@849] - Notification time out: 12800

```

关闭防火墙即可

```
[root@node1 bin]# service iptables stop
iptables: Setting chains to policy ACCEPT: filter          [  OK  ]
iptables: Flushing firewall rules:                         [  OK  ]
iptables: Unloading modules:                               [  OK  ]
[root@node1 bin]# service iptables status
iptables: Firewall is not running.

```



