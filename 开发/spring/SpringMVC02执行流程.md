# SpringMVC02执行流程


###框架结构![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/17448142.jpg)###架构流程1、	用户发送请求至前端控制器DispatcherServlet
2、	DispatcherServlet收到请求调用HandlerMapping处理器映射器。3、	处理器映射器根据请求url找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。4、	DispatcherServlet通过HandlerAdapter处理器适配器调用处理器5、	执行处理器(Controller，也叫后端控制器)。6、	Controller执行完成返回ModelAndView7、	HandlerAdapter将controller执行结果ModelAndView返回给
DispatcherServlet8、	DispatcherServlet将ModelAndView传给ViewReslover视图解析器9、	ViewReslover解析后返回具体View10、	DispatcherServlet对View进行渲染视图（即将模型数据填充至视图中）。11、	DispatcherServlet响应用户组件说明
以下组件通常使用框架提供实现：υ	DispatcherServlet：前端控制器用户请求到达前端控制器，它就相当于mvc模式中的c，dispatcherServlet是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。υ	HandlerMapping：处理器映射器HandlerMapping负责根据用户请求找到Handler即处理器，springmvc提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/42268746.jpg)υ	Handler：处理器Handler 是继DispatcherServlet前端控制器的后端控制器，在DispatcherServlet的控制下Handler对具体的用户请求进行处理。由于Handler涉及到具体的用户业务请求，所以一般情况需要程序员根据业务需求开发Handler。υ	HandlAdapter：处理器适配器通过HandlerAdapter对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。       υ	View Resolver：视图解析器View Resolver负责将处理结果生成View视图，View Resolver首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View视图对象，最后对View进行渲染将处理结果通过页面展示给用户。 υ	View：视图springmvc框架提供了很多的View视图类型的支持，包括：jstlView、freemarkerView、pdfView等。我们最常用的视图就是jsp。一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。>说明：在springmvc的各个组件中，处理器映射器、处理器适配器、视图解析器称为springmvc的三大组件。需要用户开放的组件有handler、view框架默认加载组件![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/51402764.jpg)注解映射器和适配器组件扫描器	使用组件扫描器省去在spring容器配置每个controller类的繁琐。使用<context:component-scan>自动扫描标记@controller的控制器类，配置如下：```
<!-- 扫描controller注解,多个包中间使用半角逗号分隔 -->	<context:component-scan base-package="cn.itcast.springmvc.controller.first"/>	
```RequestMappingHandlerMapping	注解式处理器映射器，对类中标记@ResquestMapping的方法进行映射，根据ResquestMapping定义的url匹配ResquestMapping标记的方法，匹配成功返回HandlerMethod对象给前端控制器，HandlerMethod对象中封装url对应的方法Method。 从spring3.1版本开始，废除了DefaultAnnotationHandlerMapping的使用，推荐使用RequestMappingHandlerMapping完成注解式处理器映射。配置如下：```
<!--注解映射器 -->	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
	
```	注解描述：@RequestMapping：定义请求url到处理器功能方法的映射RequestMappingHandlerAdapter注解式处理器适配器，对标记@ResquestMapping的方法进行适配。从spring3.1版本开始，废除了AnnotationMethodHandlerAdapter的使用，推荐使用RequestMappingHandlerAdapter完成注解式处理器适配。配置如下：```
<!--注解适配器 -->	<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
	
```###\<mvc:annotation-driven>
springmvc使用<mvc:annotation-driven>自动加载RequestMappingHandlerMapping和RequestMappingHandlerAdapter，可用在springmvc.xml配置文件中使用<mvc:annotation-driven>替代注解处理器和适配器的配置。视图解析器在springmvc.xml文件配置如下：```
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">		<property name="viewClass"			value="org.springframework.web.servlet.view.JstlView" />		<property name="prefix" value="/WEB-INF/jsp/" />		<property name="suffix" value=".jsp" />	</bean>
```InternalResourceViewResolver：支持JSP视图解析
**viewClass**：JstlView表示JSP模板页面需要使用JSTL标签库，所以classpath中必须包含jstl的相关jar 包。此属性可以不设置，默认为JstlView。**prefix 和suffix**：查找视图页面的前缀和后缀，最终视图的址为：前缀+逻辑视图名+后缀，逻辑视图名需要在controller中返回ModelAndView指定，比
如逻辑视图名为hello，则最终返回的jsp视图地址 “WEB-INF/jsp/hello.jsp”


<!--
create time: 2018-01-14 21:26:30
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

