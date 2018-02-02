##linux下expect环境安装



最简单的安装方法：

	yum install -y expect


安装包安装方法

```
gunzip expect.tar.gz

tar -xvf expect.tar
```

首先安装gcc

error: no acceptable cc found in $PATH  

```
yum install gcc
```

expect是交互性很强的脚本语言，可以帮助运维人员实现批量管理成千上百台服务器操作，是一款很实用的批量部署工具！
expect依赖于tcl，而linux系统里一般不自带安装tcl，所以需要手动安装

下载：expect-5.43.0.tar和tcl8.4.11-src.tar

[下载地址](https://pan.baidu.com/s/1kVyeLt9)  提取密码：af9p

将expect和tcl的软件包下载放到/usr/local/src目录下

（1）解压tcl，进入tcl解压目录，然后进入unix目录进行编译安装

```
[root@xw4 src]# tar -zvxf tcl8.4.11-src.tar.gz
[root@xw4 src]# cd tcl8.4.11/unix
[root@xw4 unix]# ./configure
[root@xw4 unix]# make && make install
```

（2）安装expect

```
[root@xw4 src]# tar -zvxf expect-5.43.0.tar.gz
[root@xw4 src]# cd expect-5.43.0
[root@xw4 expect-5.43.0]# ./configure --with-tclinclude=/usr/local/src/tcl8.4.11/generic --with-tclconfig=/usr/local/lib/
[root@xw4 expect-5.43.0]# make && make install
```


(3)安装完成后进行测试

```
[root@xw4 ~]# expect
expect1.1>
expect1.1>
```


建立软连接

如上expect安装后的路径是：

```
[root@node1 ~]# which expect
/usr/local/bin/expect
```
做个expect执行文件的软件

```
[root@node1 ~]# ln -s /usr/local/bin/expect /usr/bin/expect
[root@node1 ~]# ll /usr/bin/expect
```

[参考链接](https://www.cnblogs.com/kevingrace/p/5900303.html)