# Jedis入门1

###Maven依赖

```
<!--reids依赖-->
    <dependencies>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.8.0</version>
        </dependency>

			<!--Junit依赖-->
        <!-- https://mvnrepository.com/artifact/junit/junit -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>
    
```


代码：

```
<!--第一种连接方式，不推荐-->
    @Test
    public void connect1(){
        // 设置IP地址和端口        
        Jedis jedis = new Jedis("192.168.25.130", 6379);
        // 保存数据
        jedis.set("company","北京科技");
        // 获取数据
        String value = jedis.get("company");
        System.out.println(value);
        //  释放资源
        jedis.close();;
    }
    

<!--第二种连接方式，-->
	@Test
    public void test2() {

        // 获得连接池的配置对象
        JedisPoolConfig config = new JedisPoolConfig();
        //   设置最大连接数
        config.setMaxTotal(20);
        //  设置最大空闲连接
        config.setMaxIdle(10);

        // 获得连接池
        JedisPool pool = new JedisPool(config, "192.168.25.130", 6379);

        Jedis jedis = null;

        try {
            jedis = pool.getResource();
            jedis.set("class", "01班");
            System.out.println(jedis.get("class"));

        } catch (Exception e) {
            if (jedis != null) {
                jedis.close();
            }
            if (pool != null) {
                pool.close();
            }
        }
    }
```


在redis3.2之后，redis增加了protected-mode，在这个模式下，即使注释掉了bind 127.0.0.1，再访问redisd时候还是报错，如下

```
DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication password is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from external computers to Redis you may adopt one of the following solutions: 1) Just disable protected mode sending the command 'CONFIG SET protected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redis is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent. 2) Alternatively you can just disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then restarting the server. 3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 4) Setup a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start accepting connections from the outside.
```
解决办法：修改redis.conf；
设置protected-mode为no

```
protected-mode no

```

<!--
create time: 2018-03-14 15:45:37
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

