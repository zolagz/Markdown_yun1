# Spring Task

spring task定时调度，配置简单，无法集群，需单独部署



```

?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:tx="http://www.springframework.org/schema/tx"
        xmlns:aop="http://www.springframework.org/schema/aop"
        xmlns:context="http://www.springframework.org/schema/context"
        xmlns:mvc="http://www.springframework.org/schema/mvc"
        xmlns:task="http://www.springframework.org/schema/task"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
                    http://www.springframework.org/schema/tx
                    http://www.springframework.org/schema/tx/spring-tx-4.1.xsd
                    http://www.springframework.org/schema/aop
                    http://www.springframework.org/schema/aop/spring-aop-4.1.xsd
                    http://www.springframework.org/schema/context
                    http://www.springframework.org/schema/context/spring-context-4.1.xsd
                    http://www.springframework.org/schema/mvc
                    http://www.springframework.org/schema/mvc/spring-mvc-4.1.xsd
                    http://www.springframework.org/schema/task
                    http://www.springframework.org/schema/task/spring-task-4.1.xsd">
<!-- 定时器开关 -->
<task:annotation-driven />
<!-- 定义定时任务 -->
<bean id="myTaskXml" class="cn.sunmeng.demo.demo1.service.MyTaskXml" />
<!-- 任务触发时间配置 -->
<task:scheduled-tasks>
    <task:scheduled ref="myTaskXml" method="executeTask" cron="*/5 * * * * ?"/>
</task:scheduled-tasks>
</beans>


```

注意事项：

![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/32674475.jpg)


java代码

```
package cn.sunmeng.demo.demo1.service;

public class MyTaskXml {
    /**
    * 定时任务
    */
    public void executeTask(){
        System.out.println("spring 定时任务执行 executeTask() 方法。。。");
    }
}


```

<!--
create time: 2018-01-14 18:23:58
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

