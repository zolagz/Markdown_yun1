####Spring的四大特性

第一特性：IOC

1：IOC：inversion of  controller 控制反转
什么是控制反装：说白了就是将创建对象的过程或者说创建对象的权限交给了spring框架来

帮我们处理，我们再不用通过new的方式来创建Javabean对象，这个过程就叫做控制反转。DI：dependency injection 依赖注入。说白了就是使用spring框架为我们的

JavaBean的属性赋值的过程IOC与DI的区别：1：ioc是为我们创建对象。Di是我们创建对象后的属性赋值2：先有IOC，才能够使用DI1：创建对象第一种方法，使用默认构造1.1：申明UserBean
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn75yo7b5gj20sv0dztat.jpg)1.2配置我们的UserBean![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76jcpa6uj21c208facs.jpg)1.3从容器中获取我们的JavaBean对象
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76k50cq0j211j0bzgor.jpg)2：创建对象的第二种方式，使用静态工厂2.1：创建JavaBean对象![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76kphrtjj20rq060dgq.jpg)2.2：创建bean的工厂类![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76kzoc0vj20n20bodh4.jpg)2.3配置我们的JavaBean![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76lirvtzj21c2046dhe.jpg)2.4获取我们的JavaBean![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76ls024vj212o09jgo1.jpg)3：创建容器的第三种方式，使用实例工厂方式第一步：创建JavaBean对象
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76m6cv0qj21c20catb8.jpg)第二步：创建JavaBean对象的工厂类
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76mljj86j20zi0ifdib.jpg)第三步：配置我们的JavaBean
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76mv8cwfj21c207w0wm.jpg)第四步：获取我们的JavaBean![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76n6z5isj21c20agn0i.jpg)4：Bean的作用域与作用范围Bean的作用域范围有四种：
默认使用的是singleton这个范围。
Prototype：多例，每次调用都会创建一个新的实例Singleton:单例，创建一个对象，每次调用，都使用这个对象Request:适用于web开发当中，将我们的对象存储在request域中Session: 适用于web开发当中，将我们的对象存储在session中

![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76nxx0ntj21c20a2gpj.jpg)5：JavaBean的生命周期可以配置init-method与destroy-method来实现bean的初始化和关闭的时候调用指定的方法
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76orv1kcj21c204l0ur.jpg)五：spring的属性注入之DI第一种方式：使用setter方法来进行赋值第一步：创建JavaBean创建person类，并且给定属性userName，并且一定要为userName属性生成setter方法。
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76p2nd0dj21c20p2aer.jpg)第二步：申明JavaBean![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76plg1vaj21c206ntbn.jpg)第三步：调用我们的JavaBean属性
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76pwabfrj21c207u76z.jpg)第二种方式：使用构造器来申明JavaBean第一步：定义javaBean对象，并生成带所有属性的构造方法![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76qicmg6j21c20vsn5f.jpg)第二步：申明JavaBean对象![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76rg1pkkj21c20cewh8.jpg)Javabean申明的另外一种写法![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76rutaqtj21c205l415.jpg)第三步：调用JavaBean的属性
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76saxhpyj21c208iace.jpg)P名称空间与C名称空间P名称空间与C名称空间其实都是不存在的虚拟空间，主要是用于简化我们的spring为JavaBean属性赋值时的配置。第一步添加P名称空间与C名称空间到schema    xmlns:p="http://www.springframework.org/schema/p"    xmlns:c="http://www.springframework.org/schema/c"![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76sst7nnj21c20ba45h.jpg)第二步：使用P名称空间或者C名称空间来简化我们的开发![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76tf3vsrj21c20ic7b2.jpg)
SPEL表达式Spel：类似于jstl与el表达式的语言，spring可以支持我们在为属性赋值的时候，通过spel表达式来进行更改我们的属性值![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76tymwnlj21c20n9k23.jpg)集合属性赋值第一种：list集合属性赋值
第一步：创建JavaBean对象，比设置list属性，而且list属性一定要给setter方法
![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76uvn0wkj21080w7wlv.jpg)

第二步：申明JavaBean，并且为我们的list属性进行赋值![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76vmoggpj21c20e8q6c.jpg)第二种：set集合属性赋值![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76wugg1vj21c213mk1p.jpg)


![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76xdwombj21c20so10i.jpg)

第三种：map集合属性赋值![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76y79bfhj21c216c13x.jpg)

![](https://ws1.sinaimg.cn/large/965d9c07gy1fn76yj7okoj21c20q17az.jpg)