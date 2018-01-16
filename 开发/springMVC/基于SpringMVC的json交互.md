# 基于SpringMVC的json交互

###@RequestBody作用：@RequestBody注解用于读取http请求的内容(字符串)，通过springmvc提供的HttpMessageConverter接口将读到的内容转换为json、xml等格式的数据并绑定到controller方法的参数上。List.action?id=1&name=zhangsan&age=12


###@ResponseBody作用：该注解用于将Controller的方法返回的对象，通过HttpMessageConverter接口转换为指定格式的数据如：json,xml等，通过Response响应给客户端


```
<dependencies>
		
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
		
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>4.2.4.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet.jsp.jstl</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
		
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
</project>



```


Springmvc默认用MappingJacksonHttpMessageConverter对json数据进行转换，需要加入jackson的包。。，如下：```
		<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-core -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-core</artifactId>
			<version>2.8.8</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.8.8</version>
		</dependency>
		<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-annotations -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-annotations</artifactId>
			<version>2.8.8</version>
		</dependency>
```

web.xml配置；

```

<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://java.sun.com/xml/ns/javaee"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
	version="2.5">
	<display-name>SpringMVC01</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.htm</welcome-file>
		<welcome-file>default.jsp</welcome-file>
	</welcome-file-list>
	
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
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>

</web-app>


```

###配置json转换器在注解适配器中加入messageConverters

**注意：如果使用\<mvc:annotation-driven /> 则不用定义下边的内容。**

```
<!--注解适配器 -->	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">		<property name="messageConverters">		<list>		<bean class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter"></bean>		</list>		</property>	</bean>
```


代码

```

import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.servlet.ModelAndView;
import pojo.Items;

@Controller
public class ItemController {

	/**
	 * 在springMVC当中，通过ModelAndView这个对象来表示模型和视图的一个映射关系
	 * @return
	 * @RequestMapping  这个注解的作用就是将我们的请求与我们执行的方法进行映射
	 */
	@RequestMapping("/itemList.do")
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
	
	@ResponseBody
	@RequestMapping("/hello.do")
	public String haveTest() {
		return "helloworld...api";
	}
	
	@ResponseBody
	@RequestMapping("/item.do")
	public Items haveTest1() {
		return new Items(1, "zhang san", 12.01, new Date(), "this is a text");
	}

	@RequestMapping("/json.do")
    @ResponseBody
    public List<String> testJson(){
      
        List<String> cList = new ArrayList<String>();
        cList.add("Hello");
        cList.add("Hello1");
        cList.add("Hello3");
        return cList;
    }
}

```

测试结果如下：
![](http://p2ehgqigv.bkt.clouddn.com/18-1-16/77795899.jpg)



扩展阅读
[SpringMVC映射的前端后台数据交互总结](https://www.jianshu.com/p/fc8793f430f4)



<!--
create time: 2018-01-16 18:34:30
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

