# SSM三大框架整合

需求，实现从数据库当中查询出商品列表信息，展现到页面上开发环境：srpingMVC 4.2.4  + spring 4.2.4+ mybatis 3.2.7+maven整合开发我们的商品列表页


第一步：maven项目创建（war包）

第二步：解决jdk版本过低问题以及web.xml丢失问题

第三步：导入SpringMVC依赖的jar包

```
 <dependencies>	<dependency>	    <groupId>org.springframework</groupId>	    <artifactId>spring-context</artifactId>	    <version>4.2.4.RELEASE</version>	</dependency> 	<dependency>	    <groupId>org.springframework</groupId>	    <artifactId>spring-core</artifactId>	    <version>4.2.4.RELEASE</version>	</dependency>	<dependency>	    <groupId>org.springframework</groupId>	    <artifactId>spring-beans</artifactId>	    <version>4.2.4.RELEASE</version>	</dependency>	<dependency>	    <groupId>org.springframework</groupId>	    <artifactId>spring-expression</artifactId>	    <version>4.2.4.RELEASE</version>	</dependency>		<dependency>	    <groupId>org.springframework</groupId>	    <artifactId>spring-aspects</artifactId>	    <version>4.2.4.RELEASE</version>	</dependency>		<dependency>	    <groupId>org.springframework</groupId>	    <artifactId>spring-context-support</artifactId>	    <version>4.2.4.RELEASE</version>	</dependency>	<dependency>	    <groupId>org.springframework</groupId>	    <artifactId>spring-webmvc</artifactId>	    <version>4.2.4.RELEASE</version>	</dependency>		<dependency>	    <groupId>javax.servlet.jsp.jstl</groupId>	    <artifactId>jstl</artifactId>	    <version>1.2</version>	</dependency>	<dependency>	    <groupId>org.springframework</groupId>	    <artifactId>spring-tx</artifactId>	    <version>4.2.4.RELEASE</version>	</dependency>	 	 	 	 <dependency>        <groupId>commons-logging</groupId>        <artifactId>commons-logging</artifactId>        <version>1.2</version>    </dependency>			<dependency>			<groupId>javax.servlet</groupId>			<artifactId>servlet-api</artifactId>			<version>3.0-alpha-1</version>			<scope>provided</scope>		</dependency>				<dependency>			<groupId>javax.servlet</groupId>			<artifactId>jstl</artifactId>			<version>1.2</version>		</dependency>		<dependency>			<groupId>javax.servlet.jsp</groupId>			<artifactId>jsp-api</artifactId>			<version>2.1</version>			<scope>provided</scope>		</dependency>  </dependencies>

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


<!--
create time: 2018-01-16 09:31:22
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

