###linux 使用

####修改root用户密码

```
# password root

```

文件压缩与解压

```
解压 tar zxvf 文件名.tar.gz

压缩 tar zcvf 文件名.tar.gz 目标名

```
将tgz文件解压到指定目录

	tar  zxvf  /source/kernel.tgz  -C /source/ linux-2.6.29



#远程主机间的文件复制

scp 文件 主机ID:/usr/local/dev

![](http://p2ehgqigv.bkt.clouddn.com/18-2-1/5615871.jpg)

查看如下：

![](http://p2ehgqigv.bkt.clouddn.com/18-2-1/63776057.jpg)


###yum下载文件的存放位置

```
默认是： /var/cache/yum

也可以在 /etc/yum.conf 指定

cachedir=/var/cache/yum #存放目录
keepcache=1 ＃1为保存 0为不保存

metadata_expire=1800 ＃过期时间  
```


###centos7主机重命名

```
hostnamectl sethostname jamson
```
然后退出主机，重新登录，即可
[参考链接 ](http://www.linuxidc.com/Linux/2017-03/141355.htm)



解决ssh连接慢

在linux中，默认就是开启了SSH的反向DNS解析,这个会消耗大量时间，因此需要关闭。

\# vi /etc/ssh/sshd_config

UseDNS=no

在配置文件中，虽然UseDNS yes是被注释的，但默认开关就是yes

vi 下查找字符串方法。直接/查找的串即可


修改之后记得重启sshd服务

\# service sshd restart

👆[参考链接](http://blog.csdn.net/doiido/article/details/43793391)


![](http://p2ehgqigv.bkt.clouddn.com/18-4-14/44815448.jpg)

##查看开发的端口

netstat -nltp 







