# 安装Redis


Redis是c语言开发的。
安装redis需要c语言的编译环境。
如果没有gcc需要在线安装，

```
yum install -y gcc-c++

```
安装步骤：网盘[下载redis](https://pan.baidu.com/s/1dFRofnz)

第一步：redis的源码包上传到linux系统。

第二步：解压缩redis。

```
tar xzf redis-4.0.6.tar.gz -C /usr/local/dev/ 

```

￼![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/93043168.jpg)

第三步：编译。make 

```
[root@localhost redis]# cd redis-4.0.6

[root@localhost redis]# make

```
第四步：安装

make install PREFIX=/usr/local/dev/redis

```
[root@localhost redis-4.0.6]# make install PREFIX=/usr/local/dev/redis
```
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/10978241.jpg)

然后进入指定的安装目录

```
[root@localhost redis-4.0.6]# cd /usr/local/dev/redis/
[root@localhost redis]# ls
bin
```
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/39951880.jpg)


###基本配置

把/usr/local/dev/redis-4.0.6目录下的redis.conf文件拷贝到/usr/local/dev/redis目录下。

![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/59414314.jpg)

然后查看redis目录

![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/23833126.jpg)

然后就可以根据需求修改redis的配置文件了！


###后端启动

vi redis.conf 设置 daemonize yes 


![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/95221801.jpg)

然后通过加载配置文件启动reids

![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/91194905.jpg)


###启动redis
 ```
 ./bin/redis-server  ./redis.conf  
 ```
###连接redis的客户端

```
./bin/redis-cli 

```
![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/1100874.jpg)

###查看redis启动情况

ps -ef | grep -i redis

###关闭redis

进入redis的bin目录下 执行命令

```
 ./redis-cli shutdown
```

![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/18105010.jpg)

附：![](http://p2ehgqigv.bkt.clouddn.com/18-3-14/21408052.jpg)

---------------------------------------------------------

绑定地址：如果需要远程访问，可将此行注释

```
bind 127.0.0.1
```
端口，默认为6379

```
port 6379
```


####是否以守护进程运行(后端启动方式daemonize yes)

* 如果以守护进程运行，则不会在命令行阻塞，类似于服务

* 如果以非守护进程运行，则当前终端被阻塞，无法使用

* 推荐改为yes，以守护进程运行

```
daemonize no|yes
```

####数据文件

```
dbfilename dump.rdb
```



数据文件存储路径

```
dir的默认值为./，表示当前目录
推荐改为：dir /var/lib/redis

```


使用配置文件方式启动

直接运行redis-server会直接运行，阻塞当前终端
一般配置文件都放在/etc/目录下

```
sudo cp /usr/local/redis/redis.conf /etc/redis/
```
推荐指定配置文件启动

```
sudo redis-server /etc/redis/redis.conf
```
####停止redis服务

```
ps ajx|grep redis
sudo kill -9 redis的进程id
```

第五步：启动redis

a)     redis前端启动
```
[root@localhost bin]# ./redis-server

```
￼![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/44685796.jpg)

b)    redis后台启动

把解压后的redis-3.0.0/redis.conf复制到/usr/local/redis/bin目录下

```
[root@localhost redis-3.0.0]# cp redis.conf /usr/local/redis/bin/
```
![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/70600255.jpg)
修改配置文件：
￼![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/70669502.jpg)

￼![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/42042580.jpg)


保存退出，启动redis

```
[root@localhost bin]# ./redis-server redis.conf
```
查看redis进程：

```
[root@localhost bin]# ps aux|grep redis
```
###Redis-cli连接redis

[root@localhost bin]# ./redis-cli 
默认连接localhost运行在6379端口的redis服务。

```
[root@localhost bin]# ./redis-cli -h 192.168.25.153 -p 6379

-h：连接的服务器的地址

-p：服务的端口号
```

1.7.   Redis集群的搭建
至少三个节点，为保证每个节点高可用，每个节点要有备份机，故至少六台redis服务器搭建redis伪分布式，在一台虚拟机上运行6个redis实例，需要修改redis端口号复制redis/bin下的所有文件到集群目录下：

	[root@localhost local]# cp redis/bin redis-cluster/redis01 -r
	[root@localhost redis01]# ls
	dump.rdb         redis-check-aof   redis-cli   redis-sentinel
	redis-benchmark  redis-check-dump  redis.conf  redis-server
	[root@localhost redis01]# rm dump.rdb 
rm：是否删除普通文件 "dump.rdb"？y

删除redis的数据库，存在数据库时无法建立集群环境

```
[root@localhost redis01]# ls
redis-benchmark  redis-check-dump  redis.conf      redis-server
redis-check-aof  redis-cli         redis-sentinel
[root@localhost redis01]# cd ..

```
复制出六份redis
[root@localhost redis-cluster]# cp redis01/ redis06/ -r

￼


修改每份redis的配置文件，redis.conf，改端口号，打开集群配置cluster-enabled
￼

￼


这里默认是注释掉的，去除#号注释，打开redis集群功能

￼

<!--
create time: 2018-01-14 15:19:20
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

