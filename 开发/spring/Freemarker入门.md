# Freemarker入门

FreeMarker 是一个用 Java 语言编写的模板引擎，它基于模板来生成文本输出。FreeMarker与 Web 容器无关，即在 Web 运行时，它并不知道 Servlet 或 HTTP。它不仅可以用作表现层的实现技术，而且还可以用于生成 XML，JSP 或 Java 等。![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/37224736.jpg)1.3 Freemarker入门小DEMO
1.3.1工程引入依赖```
<dependency>		<groupId>org.freemarker</groupId>		<artifactId>freemarker</artifactId>		<version>2.3.23</version></dependency>  ```
1.3.2创建模板文件
>模板文件中四种元素>
> 1、文本，直接输出的部分>
>  2、注释，即<#--...-->格式不会输出>
>  3、插值（Interpolation）：即${..}部分,将使用数据模型中的部分替代输出>  4、FTL指令：FreeMarker指令，和HTML标记类似，名字前加#予以区分，不会输出。我们现在就创建一个简单的创建模板文件test.ftl```
<html><head>	<meta charset="utf-8">	<title>Freemarker入门小DEMO </title></head><body><#--我只是一个注释，我不会有任何输出  -->${name},你好。${message}</body></html>```
这里有文本、插值和注释

1.3.3生成文件
使用步骤：第一步：创建一个 Configuration 对象，直接 new 一个对象。构造方法的参数就是 
freemarker的版本号。第二步：设置模板文件所在的路径。第三步：设置模板文件使用的字符集。一般就是 utf-8.第四步：加载一个模板，创建一个模板对象。第五步：创建一个模板使用的数据集，可以是 pojo 也可以是 map。一般是 Map。第六步：创建一个 Writer 对象，一般创建一 FileWriter 对象，指定生成的文件名。第七步：调用模板对象的 process 方法输出文件。第八步：关闭流代码：```

创建Test类 main方法如下：	     //1.创建配置类		Configuration configuration=new Configuration(Configuration.getVersion());		//2.设置模板所在的目录 		configuration.setDirectoryForTemplateLoading(new File("D:/pinyougou_work/freemarkerDemo/src/main/resources/"));		//3.设置字符集		configuration.setDefaultEncoding("utf-8");		//4.加载模板		Template template = configuration.getTemplate("test.ftl");		//5.创建数据模型		Map map=new HashMap();		map.put("name", "张三 ");		map.put("message", "欢迎来到神奇的品优购世界！");		//6.创建Writer对象		Writer out =new FileWriter(new File("d:\\test.html"));		//7.输出		template.process(map, out);		//8.关闭Writer对象		out.close();```
执行后，在D盘根目录即可看到生成的test.html ，打开看看![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/11351813.jpg)是不是有些小激动呢？  1.4 FTL指令1.4.1 assign指令此指令用于在页面上定义一个变量 （1）定义简单类型：```<#assign linkman="周先生">联系人：${linkman}```
（2）定义对象类型：
```<#assign info={"mobile":"13301231212",'address':'北京市昌平区王府街'} >电话：${info.mobile}  地址：${info.address}```
运行效果：
![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/79826472.jpg)1.4.2 include指令此指令用于模板文件的嵌套创建模板文件head.ftl```<h1>黑马信息网</h1>
```我们修改test.ftl，在模板文件中使用include指令引入刚才我们建立的模板

```<#include "head.ftl">```
1.4.3 if指令在模板文件上添加```<#if success=true>  你已通过实名认证<#else>    你未通过实名认证</#if>```
在代码中对str变量赋值

```map.put("success", true);```
在freemarker的判断中，可以使用= 也可以使用== 1.4.4 list指令需求，实现商品价格表，如下图：![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/14846636.jpg)
（1）代码中对变量goodsList赋值```
		List goodsList=new ArrayList();
				Map goods1=new HashMap();		
		goods1.put("name", "苹果");		
		goods1.put("price", 5.8);		
		Map goods2=new HashMap();		
		goods2.put("name", "香蕉");		
		goods2.put("price", 2.5);		
		Map goods3=new HashMap();		
		goods3.put("name", "橘子");		
		goods3.put("price", 3.2);		
		goodsList.add(goods1);		
		goodsList.add(goods2);		
		goodsList.add(goods3);		
		map.put("goodsList", goodsList);

```（2）在模板文件上添加

```----商品价格表----<br><#list goodsList as goods>  ${goods_index+1} 商品名称： ${goods.name} 价格：${goods.price}<br></#list>

```如果想在循环中得到索引，使用循环变量+_index就可以得到。1.5 内建函数内建函数语法格式： **变量+?+函数名称**  1.5.1获取集合大小我们通常要得到某个集合的大小，如下图：![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/85097581.jpg)
我们使用size函数来实现，代码如下：>共  ${goodsList?size}  条记录
1.5.2转换JSON字符串为对象我们通常需要将json字符串转换为对象，那如何处理呢？看代码
```
    <#assign text="{'bank':'工商银行','account':'10101920201920212'}" />	<#assign data=text?eval />	开户行：${data.bank}  账号：${data.account}
	
```1.5.3日期格式化代码中对变量赋值：dataModel.put("today", new Date());在模板文件中加入当前日期：${today?date} <br>当前时间：${today?time} <br>   当前日期+时间：${today?datetime} <br>        日期格式化：  ${today?string("yyyy年MM月")}运行效果如下：1.5.4数字转换为字符串代码中对变量赋值：map.put("point", 102920122);修改模板：累计积分：${point}页面显示：我们会发现数字会以每三位一个分隔符显示，有些时候我们不需要这个分隔符，就需要将数字转换为字符串,使用内建函数c累计积分：${point?c}页面显示效果如下：1.6空值处理运算符如果你在模板中使用了变量但是在代码中没有对变量赋值，那么运行生成时会抛出异常。但是有些时候，有的变量确实是null，怎么解决这个问题呢？1.6.1判断某变量是否存在:“??”用法为:variable??,如果该变量存在,返回true,否则返回false <#if aaa??>  aaa变量存在<#else>  aaa变量不存在</#if>1.6.2缺失变量默认值:“!”我们除了可以判断是否为空值，也可以使用!对null值做转换处理在模板文件中加入>${aaa!'-'}
在代码中不对aaa赋值，也不会报错了 ，当aaa为null则返回！后边的内容-
###1.7运算符1.7.1算数运算符FreeMarker表达式中完全支持算术运算,FreeMarker支持的算术运算符包括:+, - , * , / , %1.7.2逻辑运算符逻辑运算符有如下几个: 逻辑与:&& 逻辑或:|| 逻辑非:! 逻辑运算符只能作用于布尔值,否则将产生错误 
###1.7.3比较运算符表达式中支持的比较运算符有如下几个: 1  =或者==:判断两个值是否相等. 2  !=:判断两个值是否不等. 3  >或者gt:判断左边值是否大于右边值 4  >=或者gte:判断左边值是否大于等于右边值 5  <或者lt:判断左边值是否小于右边值 6  <=或者lte:判断左边值是否小于等于右边值 注意:  =和!=可以用于字符串,数值和日期来比较是否相等,但=和!=两边必须是相同类型

的值,否则会产生错误,而且FreeMarker是精确比较,"x","x ","X"是不等的.其它的运

行符可以作用于数字和日期,但不能作用于字符串,大部分的时候,使用gt等字母运算符代替
\>会有更好的效果,因为 FreeMarker会把>解释成FTL标签的结束字符,当然,也可以使用括号来避免这种情况,如:<#if (x>y)> <!--
create time: 2018-01-12 14:27:56
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

