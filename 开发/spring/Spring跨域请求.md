# Spring跨域请求

只要协议、域名、端口有任何一个不同，都被当作是不同的域。###跨域解决方案CORS


```
response.setHeader("Access-Control-Allow-Origin", "http://localhost:9105");response.setHeader("Access-Control-Allow-Credentials", "true");

```

###SpringMVC跨域注解
springMVC的版本在4.2或以上版本，可以使用注解实现跨域, 我们只需要在需要跨域的
方法上添加注解@CrossOrigin即可@CrossOrigin(origins="http://localhost:9105",allowCredentials="true")allowCredentials="true"  可以缺省

<!--
create time: 2018-01-16 09:54:50
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

