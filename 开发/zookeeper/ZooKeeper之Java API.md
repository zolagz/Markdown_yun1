# ZooKeeper之Java API

引入依赖

```
<dependency>           <groupId>org.apache.zookeeper</groupId>           <artifactId>zookeeper</artifactId>           <version>3.4.9</version></dependency>
```

入门案例代码:

```

// 初始化 ZooKeeper 实例(zk 地址、会话超时时间，与系统默认一致、watcher)        ZooKeeper zk = new ZooKeeper("node1:2181,node2:2181", 30000, new Watcher() {            @Overridepublic void process(WatchedEvent event) { System.out.println("事件类型为:" + event.getType()); System.out.println("事件发生的路径:" + event.getPath()); System.out.println("通知状态为:" +event.getState());} });zk.create("/myGirls", "性感的".getBytes("UTF-8"), Ids.OPEN_ACL_UNSAFE, CreateMode.PERSISTENT);zk.close();

```


###更多操作

```
public static void main(String[] args) throws Exception {// 初始化 ZooKeeper 实例(zk 地址、会话超时时间，与系统默认一致、watcher)        ZooKeeper zk = new ZooKeeper("node1:2181,node2:2181", 30000, new Watcher() {            @Overridepublic void process(WatchedEvent event) { 

System.out.println("事件类型为:" + event.getType()); 

System.out.println("事件发生的路径:" + event.getPath()); 

System.out.println("通知状态为:" +event.getState());} });
// 创建一个目录节点zk.create("/testRootPath", "testRootData".getBytes(), Ids.OPEN_ACL_UNSAFE,   CreateMode.PERSISTENT);// 创建一个子目录节点 zk.create("/testRootPath/testChildPathOne", "testChildDataOne".getBytes(),   Ids.OPEN_ACL_UNSAFE,CreateMode.PERSISTENT);System.out.println(new String(zk.getData("/testRootPath",false,null)));// 取出子目录节点列表System.out.println(zk.getChildren("/testRootPath",true));// 修改子目录节点数据 zk.setData("/testRootPath/testChildPathOne","modifyChildDataOne".getBytes(),-1); 

System.out.println("目录节点状态:["+zk.exists("/testRootPath",true)+"]");// 创建另外一个子目录节点 zk.create("/testRootPath/testChildPathTwo", "testChildDataTwo".getBytes(),   Ids.OPEN_ACL_UNSAFE,CreateMode.PERSISTENT);System.out.println(new String(zk.getData("/testRootPath/
testChildPathTwo",true,null))); 

// 删除子目录节点zk.delete("/testRootPath/testChildPathTwo",-1); zk.delete("/testRootPath/testChildPathOne",-1);// 删除父目录节点 zk.delete("/testRootPath",-1); zk.close();}

```


<!--
create time: 2018-03-15 13:48:59
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

