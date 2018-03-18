# JDK版本过低


```

使用maven的插件来指定我们的jdk版本
  <build>
	<plugins>
	<!-- maven的编译插件，用于指定我们jdk的版本 -->
		  <plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>

				<configuration>
					<source>1.8</source>
					<target>1.8</target>
					<encoding>UTF-8</encoding>
				</configuration>

			</plugin>
			
	</plugins>
</build>

```

<!--
create time: 2018-03-18 13:04:19
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

