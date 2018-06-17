# Ubunto 安装mongodb

mongodb 安装

第一步
下载安装包

解压

进入bin 目录

./mongod


```
alfred@Mac:~/guan/mongodb-3.6.5/bin$ ./mongod
2018-06-17T11:01:17.957+0800 I CONTROL  [initandlisten] MongoDB starting : pid=6327 port=27017 dbpath=/data/db 64-bit host=Mac
2018-06-17T11:01:17.957+0800 I CONTROL  [initandlisten] db version v3.6.5
2018-06-17T11:01:17.958+0800 I CONTROL  [initandlisten] git version: a20ecd3e3a174162052ff99913bc2ca9a839d618
2018-06-17T11:01:17.958+0800 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2g  1 Mar 2016
2018-06-17T11:01:17.958+0800 I CONTROL  [initandlisten] allocator: tcmalloc
2018-06-17T11:01:17.959+0800 I CONTROL  [initandlisten] modules: none
2018-06-17T11:01:17.959+0800 I CONTROL  [initandlisten] build environment:
2018-06-17T11:01:17.959+0800 I CONTROL  [initandlisten]     distmod: ubuntu1604
2018-06-17T11:01:17.959+0800 I CONTROL  [initandlisten]     distarch: x86_64
2018-06-17T11:01:17.960+0800 I CONTROL  [initandlisten]     target_arch: x86_64
2018-06-17T11:01:17.960+0800 I CONTROL  [initandlisten] options: {}
2018-06-17T11:01:17.961+0800 I STORAGE  [initandlisten] 
2018-06-17T11:01:17.962+0800 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2018-06-17T11:01:17.962+0800 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2018-06-17T11:01:17.962+0800 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=256M,session_max=20000,eviction=(threads_min=4,threads_max=4),config_base=false,statistics=(fast),cache_cursors=false,log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),statistics_log=(wait=0),verbose=(recovery_progress),
2018-06-17T11:01:18.622+0800 I STORAGE  [initandlisten] WiredTiger message [1529204478:622182][6327:0x7f2ab15489c0], txn-recover: Set global recovery timestamp: 0
2018-06-17T11:01:18.645+0800 I CONTROL  [initandlisten] 
2018-06-17T11:01:18.646+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-06-17T11:01:18.647+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] 
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] ** WARNING: This server is bound to localhost.
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          Remote systems will be unable to connect to this server. 
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          Start the server with --bind_ip <address> to specify which IP 
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          addresses it should serve responses from, or with --bind_ip_all to
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          bind to all interfaces. If this behavior is desired, start the
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          server with --bind_ip 127.0.0.1 to disable this warning.
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] 
2018-06-17T11:01:18.652+0800 I CONTROL  [initandlisten] 
2018-06-17T11:01:18.652+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2018-06-17T11:01:18.652+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2018-06-17T11:01:18.652+0800 I CONTROL  [initandlisten] 
2018-06-17T11:01:18.676+0800 I STORAGE  [initandlisten] createCollection: admin.system.version with provided UUID: da013db3-cbbb-4e8f-abdd-7c719ebea61f
2018-06-17T11:01:18.693+0800 I COMMAND  [initandlisten] setting featureCompatibilityVersion to 3.6
2018-06-17T11:01:18.704+0800 I STORAGE  [initandlisten] createCollection: local.startup_log with generated UUID: e31dcef5-bdaa-4e1a-a9b0-d60de06927e9
2018-06-17T11:01:18.710+0800 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/db/diagnostic.data'
2018-06-17T11:01:18.711+0800 I NETWORK  [initandlisten] waiting for connections on port 27017
2018-06-17T11:05:44.841+0800 I NETWORK  [listener] connection accepted from 127.0.0.1:51672 #1 (1 connection now open)
2018-06-17T11:05:44.842+0800 I NETWORK  [conn1] received client metadata from 127.0.0.1:51672 conn1: { application: { name: "MongoDB Shell" }, driver: { name: "MongoDB Internal Client", version: "3.6.5" }, os: { type: "Linux", name: "Ubuntu", architecture: "x86_64", version: "16.04" } }
2018-06-17T11:06:18.712+0800 I STORAGE  [thread2] createCollection: config.system.sessions with generated UUID: a4ca963d-aecf-44ad-ad78-432dca8a6448
2018-06-17T11:06:18.722+0800 I INDEX    [thread2] build index on: config.system.sessions properties: { v: 2, key: { lastUse: 1 }, name: "lsidTTLIndex", ns: "config.system.sessions", expireAfterSeconds: 1800 }
2018-06-17T11:06:18.722+0800 I INDEX    [thread2]        building index using bulk method; build may temporarily use up to 500 megabytes of RAM
2018-06-17T11:06:18.724+0800 I INDEX    [thread2] build index done.  scanned 0 total records. 0 secs

```


遇到的问题

```

alfred@Mac:~/guan/mongodb-3.6.5/bin$ ./mongod
2018-06-17T10:58:49.303+0800 I CONTROL  [initandlisten] MongoDB starting : pid=6258 port=27017 dbpath=/data/db 64-bit host=Mac
2018-06-17T10:58:49.303+0800 I CONTROL  [initandlisten] db version v3.6.5
2018-06-17T10:58:49.304+0800 I CONTROL  [initandlisten] git version: a20ecd3e3a174162052ff99913bc2ca9a839d618
2018-06-17T10:58:49.304+0800 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2g  1 Mar 2016
2018-06-17T10:58:49.304+0800 I CONTROL  [initandlisten] allocator: tcmalloc
2018-06-17T10:58:49.305+0800 I CONTROL  [initandlisten] modules: none
2018-06-17T10:58:49.305+0800 I CONTROL  [initandlisten] build environment:
2018-06-17T10:58:49.306+0800 I CONTROL  [initandlisten]     distmod: ubuntu1604
2018-06-17T10:58:49.306+0800 I CONTROL  [initandlisten]     distarch: x86_64
2018-06-17T10:58:49.306+0800 I CONTROL  [initandlisten]     target_arch: x86_64
2018-06-17T10:58:49.307+0800 I CONTROL  [initandlisten] options: {}
2018-06-17T10:58:49.307+0800 I STORAGE  [initandlisten] exception in initAndListen: IllegalOperation: Attempted to create a lock file on a read-only directory: /data/db, terminating
2018-06-17T10:58:49.308+0800 I CONTROL  [initandlisten] now exiting
2018-06-17T10:58:49.308+0800 I CONTROL  [initandlisten] shutting down with code:100

```


解决方法：

切换为root用户

mkdir -p /data/db/


然后在添加读写权限

root@Mac:/# chmod 777 -R data

```
alfred@Mac:~/guan/mongodb-3.6.5/bin$ ./mongod
2018-06-17T11:05:39.074+0800 I CONTROL  [initandlisten] MongoDB starting : pid=6377 port=27017 dbpath=/data/db 64-bit host=Mac
2018-06-17T11:05:39.075+0800 I CONTROL  [initandlisten] db version v3.6.5
2018-06-17T11:05:39.075+0800 I CONTROL  [initandlisten] git version: a20ecd3e3a174162052ff99913bc2ca9a839d618
2018-06-17T11:05:39.078+0800 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.2g  1 Mar 2016
2018-06-17T11:05:39.078+0800 I CONTROL  [initandlisten] allocator: tcmalloc
2018-06-17T11:05:39.078+0800 I CONTROL  [initandlisten] modules: none
2018-06-17T11:05:39.078+0800 I CONTROL  [initandlisten] build environment:
2018-06-17T11:05:39.078+0800 I CONTROL  [initandlisten]     distmod: ubuntu1604
2018-06-17T11:05:39.078+0800 I CONTROL  [initandlisten]     distarch: x86_64
2018-06-17T11:05:39.078+0800 I CONTROL  [initandlisten]     target_arch: x86_64
2018-06-17T11:05:39.078+0800 I CONTROL  [initandlisten] options: {}
2018-06-17T11:05:39.078+0800 I STORAGE  [initandlisten] exception in initAndListen: DBPathInUse: Unable to lock the lock file: /data/db/mongod.lock (Resource temporarily unavailable). Another mongod instance is already running on the /data/db directory, terminating
2018-06-17T11:05:39.079+0800 I CONTROL  [initandlisten] now exiting
2018-06-17T11:05:39.079+0800 I CONTROL  [initandlisten] shutting down with code:100

```

已经启动了
直接启连接

```
alfred@Mac:~/guan/mongodb-3.6.5/bin$ ./mongo
MongoDB shell version v3.6.5
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.6.5
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
        http://docs.mongodb.org/
Questions? Try the support group
        http://groups.google.com/group/mongodb-user
Server has startup warnings: 
2018-06-17T11:01:17.961+0800 I STORAGE  [initandlisten] 
2018-06-17T11:01:17.962+0800 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2018-06-17T11:01:17.962+0800 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2018-06-17T11:01:18.645+0800 I CONTROL  [initandlisten] 
2018-06-17T11:01:18.646+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2018-06-17T11:01:18.647+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] 
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] ** WARNING: This server is bound to localhost.
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          Remote systems will be unable to connect to this server. 
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          Start the server with --bind_ip <address> to specify which IP 
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          addresses it should serve responses from, or with --bind_ip_all to
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          bind to all interfaces. If this behavior is desired, start the
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] **          server with --bind_ip 127.0.0.1 to disable this warning.
2018-06-17T11:01:18.651+0800 I CONTROL  [initandlisten] 
2018-06-17T11:01:18.652+0800 I CONTROL  [initandlisten] 
2018-06-17T11:01:18.652+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2018-06-17T11:01:18.652+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2018-06-17T11:01:18.652+0800 I CONTROL  [initandlisten] 
>
```


###总结：

首先新建 /data/db

添加权限 

然后在添加读写权限

root@Mac:/# chmod 777 -R data


解压安装包

进入bin 目录下

启动服务端命令：

./mongod

启动客户端连接

./mongo






<!--
create time: 2018-06-17 11:12:39
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

