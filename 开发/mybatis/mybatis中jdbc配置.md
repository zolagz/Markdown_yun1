# mybatis中jdbc配置

/HM课程/就业班/第1阶段/SSM/day02-mybatis第一 天/视频/11、引用外部数据库定义信息来实现数据库的连接.itheima

###第一种方式

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/web_test3?characterEncoding=utf-8"/>
                <property name="username" value="root"/>
                <property name="password" value="admin"/>
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
        <mapper class="com.itcast.demo.UserMapper"></mapper>
    </mappers>
</configuration>

```

###第二种方式

```

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    <properties>
        <property name="jdbc.dricver" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbc.url" value="jdbc:mysql://localhost:3306/web_test3?characterEncoding=utf-8"></property>
        <property name="jdbc.username" value="root"></property>
        <property name="jdbc.password" value="admin"></property>
    </properties>
    
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.dricver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
        <mapper class="com.itcast.demo.UserMapper"></mapper>
    </mappers>

</configuration>

```

###第三种方式

新建一个jdbc.properties文件,内容如下：

```
jdbc.dricver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/web_test3?characterEncoding=utf-8
jdbc.username=root
jdbc.password=admin

```
然后在SqlMapConfig.xml中配置,通过properties的resource指定jdbc.properties

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <properties resource="jdbc.properties">
        <property name="jdbc.dricver" value=""></property>
        <property name="jdbc.url" value=""></property>
        <property name="jdbc.username" value="root"></property>
        <property name="jdbc.password" value="admin"></property>
    </properties>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.dricver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <mapper class="com.itcast.demo.UserMapper"></mapper>
    </mappers>

</configuration>

```

<!--
create time: 2018-04-19 13:15:23
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

