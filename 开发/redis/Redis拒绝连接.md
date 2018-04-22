

[参考链接](https://blog.csdn.net/Jexhen/article/details/76098622)

当我们利用Jedis操作服务器的Redis数据库时，需要先将远程服务器的端口（默认端口是6379）开放，命令如下：


    #/sbin/iptables -I INPUT -p tcp --dport 6379 -j ACCEPT  
    #/etc/rc.d/init.d/iptables save  

我们以为这样就能顺利地使用jedis访问服务器的redis数据库了，其实不然，我们会得到如题的提示：Connection refused，会抛出如下的异常：

```
redis.clients.jedis.exceptions.JedisConnectionException: java.net.ConnectException: Connection refused: connect

```
是因为redis默认bind 127.0.0.1，所以你会理所当然地想到去redis的配置文件redis.conf将“bind 127.0.0.1”注释掉。

认为这样就可以顺利访问了，其实还真不能解决，我们仍然会得到异常，异常的信息给我们提示了很多方法，其中有一个方法就是让我们将protected mode关闭掉。

原来是redis默认开启了protected mode，保证只有主机才能访问到。所以正确解决jedis conneciton refused的解决方案如下：

```
1） 关闭redis-server

2） 打开redis的配置文件redis.conf

3） 将配置文件中的bind 127.0.0.1注释掉

4） 找到配置文件中protected mode，默认protected mode yes，需要将其改为protected mode no

5 ）重新开启reids-server ，命令如下（前提redis.conf文件和redis-server同一个目录）：

```

>bin/redis-server redis.conf 


但是将protected mode关闭掉明显不安全，意味着任何机器都能远程访问你的redis-server，更加安全的方法有待探究。

