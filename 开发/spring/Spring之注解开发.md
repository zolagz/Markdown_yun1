spring之注解开发


第一步导入jar包；

```<!-- https://mvnrepository.com/artifact/org.springframework/spring-aop --><dependency>    <groupId>org.springframework</groupId>    <artifactId>spring-aop</artifactId>    <version>4.2.4.RELEASE</version></dependency>

```

第二步，配置schema

![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7aawld9fj21c20i77d7.jpg)

```
  	<!-- 配置通过注解的方式来使用我们的开发 -->  	<context:annotation-config/>	<!-- 配置注解扫描的包 -->	<context:component-scan base-package="cn.itcast.spring"/>
	
```

![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7adprd7xj21c20e27b4.jpg)


第四步：定义我们的JavaBean对象![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7aebos4bj21c20wd12b.jpg)


第五步：调用我们的JavaBean对象![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7aexh98kj21c20c3wha.jpg)


JavaBean属性使用注解的方式来进行赋值![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7afm5prpj21c20y8guv.jpg)


在注解开发中，如果是复杂类型的注入，一般是用@Autowired![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7ag7rg92j21c20jc77b.jpg)



在实际开发中我们的注解  @Resource(name="mycat")  等价于
@Autowired  加上   @Qualifier(value="mycat")![](https://ws1.sinaimg.cn/large/965d9c07ly1fn7agz7c6hj217f0jyq6g.jpg)


在我们的web开发当中，spring为了分层表示我们的开发架构，还衍生出了三个相同的注解，只不过用于不同层级的标识
@Controller 控制器（注入服务）  
@Service  服务 （注入dao） @Repository dao （实现dao访问）@component （把普通pojo实例化到spring容器中，相当于配置文件中的<bean id="" class=""/>）


@Component,@Service,@Controller,@Repository注解的类，并把这些类纳入进spring容器中管理。 
下面写这个是引入component的扫描组件 
<context:component-scan base-package=”com.mmnc”>    

其中base-package为需要扫描的包（含所有子包） 

  1、@Service用于标注业务层组件 
  
  2、@Controller用于标注控制层组件(如struts中的action) 
  
  3、@Repository用于标注数据访问组件，即DAO组件. 
  
  4、@Component泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。    
           
 