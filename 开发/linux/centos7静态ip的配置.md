###CentOS7静态ip的设置


###进入文件目录：
/etc/sysconfig/network-scripts/

```
    BOOTPROTO="static" #dhcp改为static   
    ONBOOT="yes" #开机启用本配置  
    IPADDR=192.168.7.106 #静态IP  
    GATEWAY=192.168.7.1 #默认网关  
    NETMASK=255.255.255.0 #子网掩码  
    DNS1=192.168.7.1 #DNS 配置  
    
```
###修改后的效果
```
    # ]# cat /etc/sysconfig/network-scripts/ifcfg-eth0  
    HWADDR="00:15:5D:07:F1:02"  
    TYPE="Ethernet"  
    BOOTPROTO="static" #dhcp改为static   
    DEFROUTE="yes"  
    PEERDNS="yes"  
    PEERROUTES="yes"  
    IPV4_FAILURE_FATAL="no"  
    IPV6INIT="yes"  
    IPV6_AUTOCONF="yes"  
    IPV6_DEFROUTE="yes"  
    IPV6_PEERDNS="yes"  
    IPV6_PEERROUTES="yes"  
    IPV6_FAILURE_FATAL="no"  
    NAME="eth0"  
    UUID="bb3a302d-dc46-461a-881e-d46cafd0eb71"  
    ONBOOT="yes" #开机启用本配置  
    IPADDR=192.168.7.106 #静态IP  
    GATEWAY=192.168.7.1 #默认网关  
    NETMASK=255.255.255.0 #子网掩码  
    DNS1=192.168.7.1 #DNS 配置  
```

重启下网络服务

```
service network restart  
```

###查看改动后的效果，Centois 7 不再使用 ifconfig 而是用 ip 命令查看网络信息。 

```
# ip addr  
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN   
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00  
    inet 127.0.0.1/8 scope host lo  
       valid_lft forever preferred_lft forever  
    inet6 ::1/128 scope host   
       valid_lft forever preferred_lft forever  
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000  
    link/ether 00:15:5d:07:f1:02 brd ff:ff:ff:ff:ff:ff  
    inet 192.168.7.106/24 brd 192.168.7.255 scope global eth0  
       valid_lft forever preferred_lft forever  
    inet6 fe80::215:5dff:fe07:f102/64 scope link   
       valid_lft forever preferred_lft forever 
       
```
![](http://p2ehgqigv.bkt.clouddn.com/18-1-25/19762141.jpg)

[参考文章](http://www.linuxidc.com/Linux/2017-06/144401.htm?hmsr=toutiao.io)