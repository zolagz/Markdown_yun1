###Spring容器快速入门案例：

####1 创建maven web工程

(勾选Create a simple project)  ==> next ==> 配置Group id 和Artifact ID

**打包方式用war**

####解决web工程的两个问题：

1. jdk版本过低问题：

解决办法：在pox.xml中添加jdk编译插件即可；

```
<build>      <plugins>          <plugin>              <groupId>org.apache.maven.plugins</groupId>              <artifactId>maven-compiler-plugin</artifactId>              <configuration>                  <source>1.8</source>                <target>1.8</target>              </configuration>          </plugin>      </plugins>  </build>```

2 .丢失web.xml的问题；

解决方案是：创建一个web.xml文件即可；内容如下

```
<?xml version="1.0" encoding="UTF-8"?><web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"	xmlns="http://java.sun.com/xml/ns/javaee"	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"	version="2.5">	<display-name>01_helloWorld</display-name>	<welcome-file-list>		<welcome-file>index.html</welcome-file>		<welcome-file>index.htm</welcome-file>		<welcome-file>index.jsp</welcome-file>		<welcome-file>default.html</welcome-file>		<welcome-file>default.htm</welcome-file>		<welcome-file>default.jsp</welcome-file>	</welcome-file-list></web-app>```

然后做一下maven更新操作


3 导入依赖的jar包

在pom.xml中添加我们的core container核心依赖

```
<dependencies>  <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->	<dependency>   		 <groupId>org.springframework</groupId>    			<artifactId>spring-context</artifactId>    			<version>4.2.4.RELEASE</version>	</dependency>  <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->	<dependency> 		<groupId>org.springframework</groupId>    		<artifactId>spring-core</artifactId>    		<version>4.2.4.RELEASE</version>	</dependency>  <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->	<dependency>    		<groupId>org.springframework</groupId>    		<artifactId>spring-beans</artifactId>    		<version>4.2.4.RELEASE</version>	</dependency>  <!-- https://mvnrepository.com/artifact/org.springframework/spring-expression -->	<dependency>    		<groupId>org.springframework</groupId>    		<artifactId>spring-expression</artifactId>    		<version>4.2.4.RELEASE</version>	</dependency></dependencies>
```




所有依赖的jar包如下：

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
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>4.2.4.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
			<scope>provided</scope>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-aop -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>4.2.4.RELEASE</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>4.2.4.RELEASE</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
		</plugins>
	</build>
```

###IOC与DI的快速入门


创建一个UserDao的接口（interface）

```
public interface UserDao {

	public void sayHello();
}
```
创建一个UserDaoImpl的实现类：

```
public class UserDaoImpl implements UserDao {
	private String userName;	
	public String getUserName() {
		return userName;
	}
	public void setUserName(String userName) {
		this.userName = userName;
	}
	@Override
	public void sayHello() {
		System.out.println("hello  world");
	}
}
```

配置Spring容器

创建一个applicationContext.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        
        <!-- bean definitions here -->
	<bean id="userDao" class="com.itcast.demo.UserDaoImpl">
		<property name="userName" value="张三"></property>
	</bean>
	
</beans>

```

>注意配置schema:配置方法：
拷贝 http://www.springframework.org/schema/beans/spring-beans.xsd  在eclipes的偏好设置操作；


创建测试类：

```
	@Test
	public void textName() {
		ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
		UserDao bean = (UserDao) context.getBean("userDao");
		bean.sayHello();
	}
```

测试结果成功啦








