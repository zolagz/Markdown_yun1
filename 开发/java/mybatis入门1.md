mybatis入门：

首先创建maven工程；

![](https://ws1.sinaimg.cn/large/965d9c07ly1fn816w49p8j20v50optca.jpg)

![](https://ws1.sinaimg.cn/large/965d9c07ly1fn815p0hrej212k0ul486.jpg)


解决web工程创建之后存在的问题第一个问题：jdk版本过低问题使用maven的插件来指定我们的jdk版本```
  <build>	<plugins>	<!-- maven的编译插件，用于指定我们jdk的版本 -->		  <plugin>				<groupId>org.apache.maven.plugins</groupId>				<artifactId>maven-compiler-plugin</artifactId>				<configuration>					<source>1.8</source>					<target>1.8</target>					<encoding>UTF-8</encoding>				</configuration>			</plugin>				</plugins></build>

```第二个问题：web.xml丢失从其他项目中拷贝一个过来即可```
<?xml version="1.0" encoding="UTF-8"?><web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"	xmlns="http://java.sun.com/xml/ns/javaee"	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"	version="2.5">	<display-name>01_helloWorld</display-name>	<welcome-file-list>		<welcome-file>index.html</welcome-file>		<welcome-file>index.htm</welcome-file>		<welcome-file>index.jsp</welcome-file>		<welcome-file>default.html</welcome-file>		<welcome-file>default.htm</welcome-file>		<welcome-file>default.jsp</welcome-file>	</welcome-file-list></web-app>```


mybatis入门
需求：通过mybatis实现对数据库的增删改查

导入mybatis依赖的jar包```
<dependencies>    <!-- mysql -->    <dependency>		<groupId>mysql</groupId>		<artifactId>mysql-connector-java</artifactId>		<version>5.1.34</version>	</dependency>		<!-- mybatis 包 -->		<dependency>			<groupId>org.mybatis</groupId>			<artifactId>mybatis</artifactId>			<version>3.2.7</version>		</dependency>		<dependency>	    <groupId>org.bgee.log4jdbc-log4j2</groupId>	    <artifactId>log4jdbc-log4j2-jdbc4.1</artifactId>	    <version>1.16</version>	</dependency>	<dependency>	    <groupId>org.slf4j</groupId>	    <artifactId>slf4j-api</artifactId>	    <version>1.7.13</version>	</dependency>	<dependency>	    <groupId>org.slf4j</groupId>	    <artifactId>slf4j-log4j12</artifactId>	    <version>1.7.5</version>	</dependency>    <!-- https://mvnrepository.com/artifact/log4j/log4j -->	<dependency>	    <groupId>log4j</groupId>	    <artifactId>log4j</artifactId>	    <version>1.2.16</version>	</dependency>  </dependencies>
```添加log4j.properties```
# Global logging configurationlog4j.rootLogger=DEBUG, stdout# Console output...log4j.appender.stdout=org.apache.log4j.ConsoleAppenderlog4j.appender.stdout.layout=org.apache.log4j.PatternLayoutlog4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n

```第一步：定义SqlMapConfig.xml 配置数据库链接参数```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE configuration  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  "http://mybatis.org/dtd/mybatis-3-config.dtd"><configuration>	<!-- 这个environments定义了我们的数据库的连接操作 -->	  <environments default="development">	    <environment id="development">	      <transactionManager type="JDBC"/>	      <dataSource type="POOLED">	        <property name="driver" value="com.mysql.jdbc.Driver"/>	        <property name="url" value="jdbc:mysql://192.168.52.250:3306/mybatis?characterEncoding=utf-8"/>	        <property name="username" value="root"/>	        <property name="password" value="admin"/>	      </dataSource>	    </environment>	  </environments>	  <!-- 通过mappers来指定加载我们对应的mapper文件 -->		  <mappers>		    <mapper resource="mapper.xml"/>		  </mappers></configuration>
```第二步：定义mapper.xml

参考：![](https://ws1.sinaimg.cn/large/965d9c07ly1fn81apft6zj21c20dr45q.jpg)

定义如下：

```<?xml version="1.0" encoding="UTF-8" ?><!DOCTYPE mapper  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  "http://mybatis.org/dtd/mybatis-3-mapper.dtd"><mapper namespace="test"></mapper>

```

创建pojo；要求和数据库参数一一对应，并生成get/set方法；


第四步：实现mybatis对数据库的操作

例1

```

@Test
	public void getUserById() throws Exception {
		
		SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
		/*
		 * 使用bulid方法传入mybatis的核心配置文件，然后才可以创建SqlSessionFactory
		 * 获得SqlSessionFactory
		 */
		SqlSessionFactory factory = builder.build(Resources.getResourceAsReader("SqlMapConfig1.xml"));
		// 获得sqSession
		SqlSession session = factory.openSession();
		/*
		 * 第一个参数：代表 sqlId  与mapper.xml中对应
		 * 第二个参数：代表传入的参数
		 */
		User user = session.selectOne("myUserID", 12);
		System.out.println(user);
		
	}


```

SqlMapConfig1.xml配置如下：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="test">

	<!-- 
	id :表示sqlId
	resultType:表示我们的结果类型
	parameterType：表示参数类型
	#{}：表示占位符   。id表示传入的参数赋值给id这个参数
	-->
	
	<select id="myUserID" resultType="com.test.d1.User"
		parameterType="int">
		select * from user where id=#{id}
	</select>
</mapper>

```
测试结果：
![](https://ws1.sinaimg.cn/large/965d9c07ly1fn82l19bsdj20xo06k406.jpg)





例2

```
public class MyBatisDemo1 {	SqlSessionFactoryBuilder builder = null;	SqlSessionFactory sqlSessionFactory = null;	SqlSession sqlSession =  null;	
	
	//在junit测试总，@Before表示在每次执行测试方法是都会先执行一次@Before修饰的方法。。。。
	//在这里是为了帮助我们创建对象。方便我们在测试方法中直接用sqlSession调用查询方法
		@Before	public void init() throws IOException{		 builder = new SqlSessionFactoryBuilder();		//通过Reources来获取我们的配置文件		InputStream resourceAsStream = Resources.getResourceAsStream("SqlMapConfig.xml");		 sqlSessionFactory = builder.build(resourceAsStream);		 sqlSession = sqlSessionFactory.openSession();	}	@After	public void after(){		sqlSession.close();	}}通过id查询数据/**	 * mybatis的入门程序，实现数据库的增删改查	 * @throws Exception	 */	@Test	public void testMybatisFirst() throws Exception {		User selectOne = sqlSession.selectOne("getUserById", 1);		String username = selectOne.getUsername();		System.out.println(username);		//关闭sqlSession		sqlSession.close();	}

```


###模糊查询

```
	/*
	 * 模糊查询
	 */
	@Test
	public void likeSelect() {
		List<User> list = session.selectList("getUserByNickName", "超级");
		for (User user : list) {
			System.out.println(user);
		}
	}

```

mapper.xml配置

```
	<select id="getUserByNickName" resultType="com.test.d1.User"
		parameterType="string">
	select * from user where nickname like '%${value}%'
	</select>
	<!--
	${value}连接符，连接传入的参数与左右两边的%。在使用简单数据类型的时候，连接符只能用value
	-->

```

测试通过；

> 注意，在配置文件中的sql语句中，like后面没有“=”。并且注意’%${value}%‘顺序


###补充
```
#{}与${}的区别

#{} 用作占位符。在接受简单数据类型是，大括号里面的参数可以随意写

${} 表示连接符。连接传入参数两边的字符串。当传入简单数据类型的时候，大括号里面的值只能写value

```


插入：

代码：

```
	@Test
	public void inserUser() {
		User user = new User();
		user.setNickname("张三22");
		user.setAge(20);
		user.setPassword("234567");
		user.setUsername("霍元甲323");
		user.setId(31);
		session.insert("inserUserID", user);
		session.commit();
		session.close();
	}

```

> 注意：在数据库操作中，除了查询不需要提交之外，增删改都需要提交


mapper.xml配置

```
	<insert id="inserUserID" parameterType="com.test.d1.User">
	insert into user (id,username,password ,nickname,age) value(#{id},#{username},#{password},#{nickname},#{age});
	</insert>
```

###自增主键返回


代码：

```
	@Test
	public void getByRetrunKey() {
		User user = new User();
		System.out.println("插入前"+user.getId());
		user.setNickname("张三1");
		user.setAge(20);
		user.setPassword("123456");
		user.setUsername("霍元甲x");
		session.insert("insertKey", user);
		session.commit();
		session.close();
		System.out.println("插入后："+user.getId());
	}

```

mapper.xml配置

```
	<insert id="insertKey" parameterType="com.test.d1.User">
	
		<selectKey keyProperty="id" resultType="java.lang.Integer"
			order="AFTER">select last_insert_id();
  		</selectKey>
	
			insert into user (username,password ,nickname,age)
			value(#{username},#{password},#{nickname},#{age});
	</insert>


```
补充说明：

```
	<!--   			插入数时候的时候实现主键返回，获取我们插入数据的主键  			keyProperty:指定我们返回的主键，封装到哪个字段当中  			resultType:表示指定我们返回的id是什么类型的  			order:表示什么时候获取我们的主键  			使用select  last_insert_id()来获取主键，适用于主键是自增长类型的。  	 -->

```


###更新操作


代码

```
	@Test
	public void updateUser() {
		User user = new User();
		user.setNickname("李四");
		user.setUsername("周杰伦");
		user.setId(31);
		session.insert("updatUserIdd", user);
		session.commit();
		session.close();
	}

```

mapper.xml	

```

	<update id="updatUserIdd" parameterType="com.test.d1.User">
	update user set username=#{username},nickname=#{nickname} where id=#{id}
	</update>


```


