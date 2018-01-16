##Spring中@Component注解,@Controller注解详解

在使用Spring的过程中，为了避免大量使用Bean注入的Xml配置文件,我们会采用Spring提供的自动扫描注入的方式,只需要添加几行自动注入的的配置,便可以完成

Service层,Controller层等等的注入配置.使用过程中,在Service层中的实现类头上加@Compopnet注解，在Controller类头加@Controller注解，便完成了配置。例如在

Controller中当我们调用某个Service时就不需要Set方法了，直接通过@Autowried 注解对Service对象进行注解即可：例如

在Controller中：

```
@Controller
@RequestMapping("/test")
public class ExampleController {
     @Autowired  
     private ExampleService service;       
}

```


复制代码

在Service中

```
@Component 
public class ExampleServiceImpl Implements ExampleService {
     @Autowired  
     private ExampleDao exampleDao;       
}
```

Spring 中的XML配置：

```
<!-- 自动扫描service,controller组件 -->
<context:component-scan base-package="com.example.service.*"/>
<context:component-scan base-package="com.example.controller.*"/>

```

　　通常，在Bean为添加@Component注解的情况下，在启动服务时，服务会提前报出以下代码中这样的异常情况下，此时应该检查相应Bean是否正确添加@Component

注解，而在Controller层中未配置@Controller的情况，启动时服务可能不会爆出异常，但是你会发现页面请求中的URL地址是正确的，当时无论如何也访问不到Controller中相对应的方法，这个时候就需要那么需要检查@Controller注解和@RequestMapping注解是否已经添加到Class上面了。

```
org.springframework.beans.factory.BeanCreationException:Error creating bean with name 'example'

No matching bean of type [com.example.ExampleService] found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency. Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}

```



自己写的类，建议使用注解开发，如果是别人提供的jar包，就要使用配置文件的方式开发，因为我们无法修改源代码，