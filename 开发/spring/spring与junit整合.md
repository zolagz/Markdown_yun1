Spring 与 junit 的整合；

第一步：导入响应的jar包

```
<!-- https://mvnrepository.com/artifact/org.springframework/spring-test --><dependency>    <groupId>org.springframework</groupId>    <artifactId>spring-test</artifactId>    <version>4.2.4.RELEASE</version>    <scope>test</scope></dependency>

```

第二步。在测试类上添加两个注解。并且制定我们的配置文件所在位置

![](https://ws1.sinaimg.cn/large/965d9c07ly1fn79pw1jlaj21c20gm42k.jpg)


测试：

![](https://ws1.sinaimg.cn/large/965d9c07ly1fn79z7alq2j20y80h6th1.jpg)