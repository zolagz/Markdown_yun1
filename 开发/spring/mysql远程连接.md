mysql运行远程连接：

第一步：登录msyql：
![](http://p2ehgqigv.bkt.clouddn.com/18-1-13/10711578.jpg)

第二步：use mysql;

第三步：执行如下命令

```
mysql>GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'admin' WITH GRANT OPTION;

```

![](http://p2ehgqigv.bkt.clouddn.com/18-1-13/51263513.jpg)

操作完后切记执行以下命令刷新权限 

```
mysql>FLUSH PRIVILEGES;
```
![](http://p2ehgqigv.bkt.clouddn.com/18-1-13/92375809.jpg)
查看修改后的用户信息

```
mysql>select host,user,password from user;
```
![](http://p2ehgqigv.bkt.clouddn.com/18-1-13/41599079.jpg)

如果密码设置错了，用下面的方法修改；

修改密码；

```
 mysql> update user set password = 'admin' where user = '%';
```
![](http://p2ehgqigv.bkt.clouddn.com/18-1-13/92985499.jpg)
 命令行下连接本地数据库：

```
 >mysql -h localhost -u root -padmin
``` 
 命令行下连接远程数据库：

```
 >mysql -h 192.168.0.201 -P 3306 -u root -padmin
```


##附录：

###删除匿名用户：

```
delete from user where user='';
```

###增加允许远程访问的用户或者允许现有用户的远程访问

```
mysql> grant all privileges on *.* to 'root'@'%' identified by '123456' with grant option;
```

###退出数据库

```
mysql> exit
```
