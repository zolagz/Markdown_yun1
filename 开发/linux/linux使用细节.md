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





###yum下载文件的存放位置

```
默认是： /var/cache/yum

也可以在 /etc/yum.conf 指定

cachedir=/var/cache/yum #存放目录
keepcache=1 ＃1为保存 0为不保存

metadata_expire=1800 ＃过期时间  
```