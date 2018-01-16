###CAS入门demo


####客户端工程1搭建

搭建工程引入依赖创建Maven工程 （war）casclient_demo1  引入cas客户端依赖并制定tomcat运行端口为9001

引入依赖

```
	<dependencies>
		<!-- cas -->  
		<dependency>  
		    <groupId>org.jasig.cas.client</groupId>  
		    <artifactId>cas-client-core</artifactId>  
		    <version>3.3.3</version>  
		</dependency>  		
		
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>  
			<scope>provided</scope>
		</dependency>
	</dependencies>  

  <build>  
	  <plugins>
	      <plugin>  
	          <groupId>org.apache.maven.plugins</groupId>  
	          <artifactId>maven-compiler-plugin</artifactId>  
	          <version>2.3.2</version>  
	          <configuration>  
	              <source>1.7</source>  
	              <target>1.7</target>  
	          </configuration>  
	      </plugin>  
	      <plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<configuration>
					<!-- 指定端口 -->
					<port>9002</port>
					<!-- 请求路径 -->
					<path>/</path>
				</configuration>
	  	  </plugin>
	  </plugins>  
    </build>

```

添加web.xml

```
<?xml version="1.0" encoding="UTF-8"?><web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"	xmlns="http://java.sun.com/xml/ns/javaee"	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"	version="2.5">	    <!-- 用于单点退出，该过滤器用于实现单点登出功能，可选配置 -->      <listener>       <listener-class>org.jasig.cas.client.session.SingleSignOutHttpSessionListener</listener-class>      </listener>      <!-- 该过滤器用于实现单点登出功能，可选配置。 -->      <filter>          <filter-name>CAS Single Sign Out Filter</filter-name>         <filter-class>org.jasig.cas.client.session.SingleSignOutFilter</filter-class>      </filter>      <filter-mapping>          <filter-name>CAS Single Sign Out Filter</filter-name>          <url-pattern>/*</url-pattern>      </filter-mapping>      <!-- 该过滤器负责用户的认证工作，必须启用它 -->      <filter>          <filter-name>CASFilter</filter-name>       <filter-class>org.jasig.cas.client.authentication.AuthenticationFilter</filter-class>          <init-param>              <param-name>casServerLoginUrl</param-name>              <param-value>http://localhost:9100/cas/login</param-value>              <!--这里的server是服务端的IP -->          </init-param>          <init-param>              <param-name>serverName</param-name>              <param-value>http://localhost:9001</param-value>        </init-param>      </filter>      <filter-mapping>          <filter-name>CASFilter</filter-name>          <url-pattern>/*</url-pattern>      </filter-mapping>      <!-- 该过滤器负责对Ticket的校验工作，必须启用它 -->      <filter>          <filter-name>CAS Validation Filter</filter-name>          <filter-class>     org.jasig.cas.client.validation.Cas20ProxyReceivingTicketValidationFilter</filter-class>          <init-param>              <param-name>casServerUrlPrefix</param-name>              <param-value>http://localhost:9100/cas</param-value>          </init-param>          <init-param>              <param-name>serverName</param-name>              <param-value>http://localhost:9001</param-value>        </init-param>      </filter>      <filter-mapping>          <filter-name>CAS Validation Filter</filter-name>          <url-pattern>/*</url-pattern>      </filter-mapping>      <!-- 该过滤器负责实现HttpServletRequest请求的包裹， 比如允许开发者通过HttpServletRequest的getRemoteUser()方法获得SSO登录用户的登录名，可选配置。 -->      <filter>          <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>          <filter-class>              org.jasig.cas.client.util.HttpServletRequestWrapperFilter</filter-class>      </filter>      <filter-mapping>          <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>          <url-pattern>/*</url-pattern>      </filter-mapping>      <!-- 该过滤器使得开发者可以通过org.jasig.cas.client.util.AssertionHolder来获取用户的登录名。 比如AssertionHolder.getAssertion().getPrincipal().getName()。 -->      <filter>          <filter-name>CAS Assertion Thread Local Filter</filter-name>       <filter-class>org.jasig.cas.client.util.AssertionThreadLocalFilter</filter-class>      </filter>      <filter-mapping>          <filter-name>CAS Assertion Thread Local Filter</filter-name>          <url-pattern>/*</url-pattern>      </filter-mapping>  </web-app>

```（3）编写index.jsp```

<%@ page language="java" contentType="text/html; charset=utf-8"    pageEncoding="utf-8"%><!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"><html><head><meta http-equiv="Content-Type" content="text/html; charset=utf-8"><title>一品优购</title></head><body>欢迎来到一品优购<%=request.getRemoteUser()%></body></html>

```
request.getRemoteUser()为获取远程登录名