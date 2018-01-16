# Springboot学习


###1.2 Spring Boot入门小Demo####1.2.1起步依赖创建Maven工程 springboot_demo（打包方式jar）  在pom.xml中添加如下依赖```  <parent>    <groupId>org.springframework.boot</groupId>    <artifactId>spring-boot-starter-parent</artifactId><version>1.4.0.RELEASE</version>  </parent>    <dependencies>    <dependency>        <groupId>org.springframework.boot</groupId>        <artifactId>spring-boot-starter-web</artifactId>    </dependency>  </dependencies>
 
```我们会惊奇地发现，我们的工程自动添加了好多好多jar包![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/69629877.jpg)而这些jar包正式我们做开发时需要导入的jar包。因为这些jar包被我们刚才引入的spring-boot-starter-web所引用了，所以我们引用spring-boot-starter-web后会自动把依赖传递过来。####1.2.2变更JDK版本我们发现默认情况下工程的JDK版本是1.6 ,而我们通常用使用1.7的版本，所以我们需要在pom.xml中添加以下配置```  <properties>       <java.version>1.7</java.version>  </properties>
 
```添加后更新工程，会发现版本已经变更为1.71.2.3引导类只需要创建一个引导类 .```package cn.itcast.demo;import org.springframework.boot.SpringApplication;import org.springframework.boot.autoconfigure.SpringBootApplication;@SpringBootApplicationpublic class Application {	public static void main(String[] args) {		SpringApplication.run(Application.class, args);	}}

```简单解释一下，@SpringBootApplication其实就是以下三个注解的总和@Configuration： 用于定义一个配置类@EnableAutoConfiguration ：Spring Boot会自动根据你jar包的依赖来自动配置
项目。@ComponentScan： 告诉Spring 哪个packages 的用注解标识的类 会被spring自动扫描并且装入bean容器。我们直接执行这个引导类，会发现控制台出现的这个标识![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/26438869.jpg)
你能不能看出来上边这个图是什么东西？####1.2.4 Spring MVC实现Hello World输出我们现在开始使用spring MVC框架，实现json数据的输出。如果按照我们原来的做法，需要在web.xml中添加一个DispatcherServlet的配置，再添加一个spring的配置文件，配置文件中需要添加如下配置```    <!-- 使用组件扫描，不用将controller在spring中配置 -->	<context:component-scan base-package="cn.itcast.demo.controller" />    <!-- 使用注解驱动不用在下边定义映射器和适配置器 -->  	<mvc:annotation-driven>	  <mvc:message-converters register-defaults="true">	    <bean class="com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter">  	      <property name="supportedMediaTypes" value="application/json"/>	      <property name="features">	        <array>	          <value>WriteMapNullValue</value>	          <value>WriteDateUseDateFormat</value>	        </array>	      </property>	    </bean>	  </mvc:message-converters>  	</mvc:annotation-driven>```但是我们用SpringBoot，这一切都省了。我们直接写Controller类

```package cn.itcast.demo.controller;import org.springframework.web.bind.annotation.RequestMapping;import org.springframework.web.bind.annotation.RestController;@RestControllerpublic class HelloWorldController {	@RequestMapping("/info")	public String info(){		return "HelloWorld";			}		}

```我们运行启动类来运行程序在浏览器地址栏输入 http://localhost:8080/info 即可看到运行结果####1.2.5修改tomcat启动端口在src/main/resources下创建application.properties
>server.port=8088重新运行引导类。地址栏输入http://localhost:8088/info####1.2.6读取配置文件信息在src/main/resources下的application.properties 增加配置>url=http://www.itcast.cn

1.2.6读取配置文件信息
在src/main/resources下的application.properties 增加配置>url=http://www.itcast.cn我要在类中读取这个配置信息，修改HelloWorldController  ```	@Autowired	private Environment env;	@RequestMapping("/info")	public String info(){		return "HelloWorld~~"+env.getProperty("url");	}```
####1.2.7热部署我们在开发中反复修改类、页面等资源，每次修改后都是需要重新启动才生效，这样每次启动都很麻烦，浪费了大量的时间，能不能在我修改代码后不重启就能生效呢？可以，在pom.xml中添加如下配置就可以实现这样的功能，我们称之为热部署。	<dependency>  	    <groupId>org.springframework.boot</groupId>  	    <artifactId>spring-boot-devtools</artifactId>  	</dependency>  赶快试试看吧，是不是很爽。####1.3 Spring Boot与ActiveMQ整合1.3.1使用内嵌服务（1）在pom.xml中引入ActiveMQ起步依赖```
<dependency>    <groupId>org.springframework.boot</groupId>    <artifactId>spring-boot-starter-activemq</artifactId></dependency>```
（2）创建消息生产者```/** * 消息生产者 * @author Administrator */@RestControllerpublic class QueueController {	@Autowired	private JmsMessagingTemplate jmsMessagingTemplate;	@RequestMapping("/send")	public void send(String text){		jmsMessagingTemplate.convertAndSend("itcast", text);	}}
```（3）创建消息消费者```
@Componentpublic class Consumer {	@JmsListener(destination="itcast")	public void readMessage(String text){		System.out.println("接收到消息："+text);	}	}```

测试：启动服务后，在浏览器执行 >http://localhost:8088/send.do?text=aaaaa
即可看到控制台输出消息提示。Spring Boot内置了ActiveMQ的服务，所以我们不用单独启动也可以执行应用程序。1.3.2使用外部服务在src/main/resources下的application.properties增加配置, 指定ActiveMQ的地址>spring.activemq.broker-url=tcp://192.168.25.135:61616
运行后，会在activeMQ中看到发送的queue 1.3.3发送Map信息（1）修改QueueController.java	@RequestMapping("/sendmap")	public void sendMap(){		Map map=new HashMap<>();		map.put("mobile", "13900001111");		map.put("content", "恭喜获得10元代金券");				jmsMessagingTemplate.convertAndSend("itcast_map",map);	}
	（2）修改Consumer.java	
	@JmsListener(destination="itcast_map")	public void readMap(Map map){		System.out.println(map);			}


<!--
create time: 2018-01-12 15:46:43
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

