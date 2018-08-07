# VMware克隆linux


注意一定要把虚拟机保持关机状态


重点是：

第一 修改主机名hostname
>vi /etc/sysconfig/network

第二 删除rules
>rm -rf /etc/udev/rules.d/70-persistent-net.rules


第三 修改IP地址

> 首先删除mac（HWADDR）地址，第二个修改ip(IPADDR)地址
> 
>vi /etc/sysconfig/network-scripts/ifcfg-eth0 
修改后如下：

```
DEVICE=eth0
TYPE=Ethernet
UUID=0fc46ca8-1898-442d-aaf7-303de311cee7
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
IPADDR=192.168.25.131
GATEWAY=192.168.25.2
NETMASK=255.255.255.0
DNS1=8.8.8.8
```

###操作步骤如下

选择右键管理 ---》克隆

![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/6910665.jpg)


![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/35848696.jpg)


![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/69856583.jpg)

![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/25589949.jpg)

![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/54744430.jpg)


![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/37808175.jpg)

![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/96860372.jpg)

![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/96860372.jpg)

![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/52681124.jpg)

![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/7498321.jpg)

###修改主机名hostname 	vi /etc/sysconfig/network![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/71786106.jpg)###删除rules文件	rm -rf /etc/udev/rules.d/70-persistent-net.rules
![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/92841534.jpg)###修改ip地址	vi /etc/sysconfig/network-scripts/ifcfg-eth0 
![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/8451564.jpg)首先删除mac地址(即：HWADDR)，第二个修改ip地址(即：IPADDR)![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/12196448.jpg)效果如下：![](http://p2ehgqigv.bkt.clouddn.com/18-1-29/17776651.jpg)验证：1、Hostname 查看主机名    2、查看ip地址15、在ping www.baidu.com 能够ping通就行

<!--
create time: 2018-01-29 20:14:20
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

