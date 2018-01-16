### Spring Data Redis入门小Demo

（1）构建Maven工程  SpringDataRedisDemo（2）引入Spring相关依赖、引入JUnit依赖   （3）引入Jedis和SpringDataRedis依赖

```

	<dependencies>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>4.2.4.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>4.2.4.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>4.2.4.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-expression -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-expression</artifactId>
			<version>4.2.4.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>4.2.4.RELEASE</version>
			<scope>test</scope>
		</dependency>
		<!-- 缓存 -->
		<dependency>
			<groupId>redis.clients</groupId>
			<artifactId>jedis</artifactId>
			<version>2.8.1</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.data</groupId>
			<artifactId>spring-data-redis</artifactId>
			<version>1.7.2.RELEASE</version>
		</dependency>
	</dependencies>


```
在src/main/resources下创建properties文件夹，建立redis-config.properties 
内容如下

```

# Redis settings 
# server IP 
redis.host=192.168.25.129
# server port 
redis.port=6379
# server pass 
redis.pass=
# use dbIndex 
redis.database=0 
# 控制一个pool最多有多少个状态为idle(空闲的)的jedis实例 
redis.maxIdle=300 
# 表示当borrow(引入)一个jedis实例时，最大的等待时间，如果超过等待时间(毫秒)，则直接抛出JedisConnectionException；  

redis.maxWait=3000 
# 在borrow一个jedis实例时，是否提前进行validate操作；如果为true，则得到的jedis实例均是可用的  
redis.testOnBorrow=true 
```
**注意👆配置文件句尾不能有空格**否则会报错

```
Caused by: redis.clients.jedis.exceptions.JedisConnectionException: Could not get a resource from the pool

Caused by: redis.clients.jedis.exceptions.JedisConnectionException: java.net.UnknownHostException: 192.168.25.129 

```

####（5）在src/main/resources下创建spring文件夹 ，创建applicationContext-redis.xml
```
<?xml version="1.0" encoding="UTF-8"?> 
<beans xmlns="http://www.springframework.org/schema/beans" 
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p" 
  xmlns:context="http://www.springframework.org/schema/context" 
  xmlns:mvc="http://www.springframework.org/schema/mvc" 
  xmlns:cache="http://www.springframework.org/schema/cache"
  xsi:schemaLocation="http://www.springframework.org/schema/beans   
            http://www.springframework.org/schema/beans/spring-beans.xsd   
            http://www.springframework.org/schema/context   
            http://www.springframework.org/schema/context/spring-context.xsd   
            http://www.springframework.org/schema/mvc   
            http://www.springframework.org/schema/mvc/spring-mvc.xsd 
            http://www.springframework.org/schema/cache  
            http://www.springframework.org/schema/cache/spring-cache.xsd">  
  
   <context:property-placeholder location="classpath*:properties/*.properties"/>   
  
   <!-- redis 相关配置 --> 
   <bean id="poolConfig" class="redis.clients.jedis.JedisPoolConfig">  
     <property name="maxIdle" value="${redis.maxIdle}" />   
     <property name="maxWaitMillis" value="${redis.maxWait}" />  
     <property name="testOnBorrow" value="${redis.testOnBorrow}" />  
   </bean>  
  
   <bean id="JedisConnectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory" 
       p:host-name="${redis.host}" p:port="${redis.port}" p:password="${redis.pass}" p:pool-config-ref="poolConfig"/>  
   
   <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">  
    	<property name="connectionFactory" ref="JedisConnectionFactory" />  
   </bean>  
     
</beans>  

```


>maxIdle ：最大空闲数>
>maxWaitMillis:连接时的最大等待毫秒数
>>testOnBorrow：在提取一个jedis实例时，是否提前进行验证操作；如果为true，则得到的jedis实例均是可用的；>

值类型操作```
@RunWith(SpringJUnit4ClassRunner.class)@ContextConfiguration(locations="classpath:spring/applicationContext-redis.xml")public class TestValue {	@Autowired	private RedisTemplate redisTemplate;		@Test	public void setValue(){		redisTemplate.boundValueOps("name").set("itcast");			}		@Test	public void getValue(){		String str = (String) redisTemplate.boundValueOps("name").get();		System.out.println(str);	}		@Test	public void deleteValue(){		redisTemplate.delete("name");;	}	}

```

###Set类型操作

```@RunWith(SpringJUnit4ClassRunner.class)@ContextConfiguration(locations="classpath:spring/applicationContext-redis.xml")public class TestSet {		@Autowired	private RedisTemplate redisTemplate;		/**	 * 存入值	 */	@Test	public void setValue(){		redisTemplate.boundSetOps("nameset").add("曹操");				redisTemplate.boundSetOps("nameset").add("刘备");			redisTemplate.boundSetOps("nameset").add("孙权");	}		/**	 * 提取值	 */	@Test	public void getValue(){		Set members = redisTemplate.boundSetOps("nameset").members();		System.out.println(members);	}		/**	 * 删除集合中的某一个值	 */	@Test	public void deleteValue(){		redisTemplate.boundSetOps("nameset").remove("孙权");	}		/**	 * 删除整个集合	 */	@Test	public void deleteAllValue(){		redisTemplate.delete("nameset");	}}```

###Set类型操作

```@RunWith(SpringJUnit4ClassRunner.class)@ContextConfiguration(locations="classpath:spring/applicationContext-redis.xml")public class TestSet {		@Autowired	private RedisTemplate redisTemplate;		/**	 * 存入值	 */	@Test	public void setValue(){		redisTemplate.boundSetOps("nameset").add("曹操");				redisTemplate.boundSetOps("nameset").add("刘备");			redisTemplate.boundSetOps("nameset").add("孙权");	}		/**	 * 提取值	 */	@Test	public void getValue(){		Set members = redisTemplate.boundSetOps("nameset").members();		System.out.println(members);	}		/**	 * 删除集合中的某一个值	 */	@Test	public void deleteValue(){		redisTemplate.boundSetOps("nameset").remove("孙权");	}		/**	 * 删除整个集合	 */	@Test	public void deleteAllValue(){		redisTemplate.delete("nameset");	}}```###（2）左压栈```
	/**	 * 左压栈：后添加的对象排在前边	 */	@Test	public void testSetValue2(){				redisTemplate.boundListOps("namelist2").leftPush("刘备");		redisTemplate.boundListOps("namelist2").leftPush("关羽");		redisTemplate.boundListOps("namelist2").leftPush("张飞");			}		/**	 * 显示左压栈集合	 */	@Test	public void testGetValue2(){		List list = redisTemplate.boundListOps("namelist2").range(0, 10);		System.out.println(list);	}

```>运行结果：>[张飞, 关羽, 刘备]
###根据索引查询元素

```	/**	 * 查询集合某个元素	 */	@Test	public void testSearchByIndex(){		String s = (String) redisTemplate.boundListOps("namelist1").index(1);		System.out.println(s);	}移除某个元素的值	/**	 * 移除集合某个元素	 */	@Test	public void testRemoveByIndex(){		redisTemplate.boundListOps("namelist1").remove(1, "关羽");	}
```###4.5.5 Hash类型操作
创建测试类TestHash（1）存入值```	@Test	public void testSetValue(){		redisTemplate.boundHashOps("namehash").put("a", "唐僧");		redisTemplate.boundHashOps("namehash").put("b", "悟空");		redisTemplate.boundHashOps("namehash").put("c", "八戒");		redisTemplate.boundHashOps("namehash").put("d", "沙僧");	}```###（2）提取所有的KEY	
```
@Test	public void testGetKeys(){		Set s = redisTemplate.boundHashOps("namehash").keys();				System.out.println(s);			}```运行结果：>[a, b, c, d]
###（3）提取所有的值

```	@Test	public void testGetValues(){		List values = redisTemplate.boundHashOps("namehash").values();		System.out.println(values);			}
```运行结果：>[唐僧, 悟空, 八戒, 沙僧]###（4）根据KEY提取值```
	@Test	public void testGetValueByKey(){		Object object = redisTemplate.boundHashOps("namehash").get("b");		System.out.println(object);	}
```运行结果：>悟空###（5）根据KEY移除值

```
	@Test	public void testRemoveValueByKey(){		redisTemplate.boundHashOps("namehash").delete("c");	}```运行后再次查看集合内容：>[唐僧, 悟空, 沙僧]