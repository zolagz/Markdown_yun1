# SpringMVC01



####springMVC的入门案例环境搭建

环境准备：jdk1.8+eclipse-MARS+springMVC

第一步：创建eclipseWEB工程 (war工程)
 ![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/75379996.jpg)

第二步：解决工程创建之后的两个问题
第一个问题：jdk版本过低问题  并配置tomcat插件

```
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
        
                <plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.2</version>
				<configuration>
					<!-- 指定端口 -->
					<port>8081</port>
					<!-- 请求路径 -->
					<path>/</path>
				</configuration>
			</plugin>
			
    </plugins>  
</build>

```

第二个问题：web.xml丢失问题
 
创建web.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">
	<display-name>01_helloWorld</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>
</web-app>

```

第三步：导入springMVC必须依赖的jar包

```
 <dependencies>
 
 <!--Spring依赖-->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context</artifactId>
	    <version>4.2.4.RELEASE</version>
	</dependency>
 	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-core</artifactId>
	    <version>4.2.4.RELEASE</version>
	</dependency>
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-beans</artifactId>
	    <version>4.2.4.RELEASE</version>
	</dependency>
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-expression</artifactId>
	    <version>4.2.4.RELEASE</version>
	</dependency>
	
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-aspects</artifactId>
	    <version>4.2.4.RELEASE</version>
	</dependency>
	
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-context-support</artifactId>
	    <version>4.2.4.RELEASE</version>
	</dependency>
	
	<!--SpiringWebMVC依赖-->
	
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-webmvc</artifactId>
	    <version>4.2.4.RELEASE</version>
	</dependency>
	
	<!--servlet之jstl依赖-->
	<dependency>
	    <groupId>javax.servlet.jsp.jstl</groupId>
	    <artifactId>jstl</artifactId>
	    <version>1.2</version>
	</dependency>

	<!--Spring 包扫描依赖-->
	<dependency>
	    <groupId>org.springframework</groupId>
	    <artifactId>spring-tx</artifactId>
	    <version>4.2.4.RELEASE</version>
	</dependency>
	 
	 <dependency>
        <groupId>commons-logging</groupId>
        <artifactId>commons-logging</artifactId>
        <version>1.2</version>
   	 </dependency>
	
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>3.0-alpha-1</version>
			<scope>provided</scope>
		</dependency>
		
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
  </dependencies>

```

第四步：需求定义
实现商品列表的静态数据展示
 



第五步：创建springmvc.xml并且进行配置

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	   xmlns:context="http://www.springframework.org/schema/context"
	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xmlns:aop="http://www.springframework.org/schema/aop"
	   xmlns:tx="http://www.springframework.org/schema/tx"
	   xmlns:p="http://www.springframework.org/schema/p"
	   xmlns:c="http://www.springframework.org/schema/c"
	   xmlns:mvc="http://www.springframework.org/schema/mvc"
	   xsi:schemaLocation="
	   		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
        	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        	http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
        	http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

	
	<context:component-scan base-package="cn.itcast.springmvc.controller"></context:component-scan>
	

</beans>

```


```

<!-- 转换器配置 -->	<bean id="conversionService"		class="org.springframework.format.support.FormattingConversionServiceFactoryBean">		<property name="converters">			<set>				<bean class="cn.itcast.springmvc.convert.DateConverter"/>			</set>		</property>	</bean>	<!-- 自定义webBinder -->	<bean id="customBinder"	class="org.springframework.web.bind.support.ConfigurableWebBindingInitializer">		<property name="conversionService" ref="conversionService" />	</bean>
	
	
		<!--注解适配器 -->	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">		 <property name="webBindingInitializer" ref="customBinder"></property> 	</bean>
		<!-- 注解处理器映射器 -->	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
	
	
	
		<!-- 加载注解驱动 -->	<!-- <mvc:annotation-driven/> -->
	
		<!-- 视图解析器 -->	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">		<property name="viewClass"			value="org.springframework.web.servlet.view.JstlView" />		<!-- jsp前缀 -->		<property name="prefix" value="/WEB-INF/jsp/" />		<!-- jsp后缀 -->		<property name="suffix" value=".jsp" />

```

![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/974891.jpg)


第六步：配置springMVC的前端控制器

```
  <!-- The front controller of this Spring Web application, responsible for handling all application requests -->
	<servlet>
		<servlet-name>springDispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
<!-- 指定springMVC的配置文件所在的路径，如果不指定，默认找 ： /WEB-INF/${servlet-name}-servlet.xml  -->
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:springmvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>

	<!-- Map all requests to the DispatcherServlet for handling -->
	<servlet-mapping>
		<servlet-name>springDispatcherServlet</servlet-name>
		<!-- 这个url-pattern表示你要拦截的哪些请求 -->
		<url-pattern>*.action</url-pattern>
	</servlet-mapping>
	
```

第七步：创建我们的ItemController对象

```
package cn.itcast.springmvc.controller;
import org.springframework.stereotype.Controller;
@Controller
public class ItemController {
}
```

定义pojo对象

```
public class Items {
	private int id;
	private String name;
	private Double price;
	private Date createtime;
	private String detail;
	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public Double getPrice() {
		return price;
	}
	public void setPrice(Double price) {
		this.price = price;
	}
	public Date getCreatetime() {
		return createtime;
	}
	public void setCreatetime(Date createtime) {
		this.createtime = createtime;
	}
	public String getDetail() {
		return detail;
	}
	public void setDetail(String detail) {
		this.detail = detail;
	}
	public Items(int id, String name, Double price, Date createtime, String detail) {
		super();
		this.id = id;
		this.name = name;
		this.price = price;
		this.createtime = createtime;
		this.detail = detail;
	}

}

```

第八步：开发我们的springMVC的代码

```
	/**
	 * 在springMVC当中，通过ModelAndView这个对象来表示模型和视图的一个映射关系
	 * @return
	 * @RequestMapping  这个注解的作用就是将我们的请求与我们执行的方法进行映射
	 */
	@RequestMapping("/itemList.action")
	public ModelAndView getItemList(){
		ModelAndView view = new ModelAndView();
		//addObject这个方法就相当于把我们的数据封装到我们的request域当中去了。
		List<Items> list = new ArrayList<Items>();
		list.add(new Items(1, "imac1", 20000d, new Date(), "买买买，毫不留情"));
		list.add(new Items(2, "imac2", 20000d, new Date(), "买买买，毫不留情"));
		list.add(new Items(3, "imac3", 20000d, new Date(), "买买买，毫不留情"));
		list.add(new Items(4, "imac4", 20000d, new Date(), "买买买，毫不留情"));
		view.addObject("itemList", list);
		//通过setViewName来指定我们要跳转的jsp页面
		view.setViewName("/WEB-INF/jsp/itemList.jsp");
		return view;
	}

```
第九步：访问测试

将我们的项目加入到我们的tomcat当中，并启动我们的tomcat
访问测试

>http://localhost:8081/SpringMVC01/itemList.action

如果通过maven配置tomcat访问路径**<path>/</path>**的话。

![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/2355803.jpg)




访问地址为

>http://localhost:8081/itemList.action


效果如下：
![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/2746570.jpg)


[项目地址](https://github.com/Denzola/SpringMVC01.git)

<!--
create time: 2018-01-14 20:20:03
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

