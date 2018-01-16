spring 与web的整合

六：spring与web整合Spring为什么要与web项目进行整合：为了在我们项目一起动的时候，就加载我们的spring容器。现在的需求，在我们的http请求中得到我们的spring容器。第一步：导入spring与web整合的对应版本的jar包```<!-- https://mvnrepository.com/artifact/org.springframework/spring-web --><dependency>    <groupId>org.springframework</groupId>    <artifactId>spring-web</artifactId>    <version>4.2.4.RELEASE</version></dependency><!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api --><dependency>    <groupId>javax.servlet</groupId>    <artifactId>javax.servlet-api</artifactId>    <version>3.1.0</version>    <scope>provided</scope></dependency>```第二步：配置监听器，监听我们的web项目的在我们的web.xml中添加监听器

```<!-- needed for ContextLoaderListener -->	<context-param>		<param-name>contextConfigLocation</param-name>		<param-value>classpath:applicationContext.xml</param-value>	</context-param>	<!-- Bootstraps the root web application context before servlet initialization -->	<listener>		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>	</listener>
	
```第三步

![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7c4k20ojj214t0pojzz.jpg)

![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7c52oa0ej21c20o7480.jpg)



第四步：设置我们的servlet访问路径
![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7c5hehvkj21c20hsn28.jpg)


第五步：页面访问，http://localhost:8080/spring_day01/servlet断点可以看得到我们的spring容器已经加载了。