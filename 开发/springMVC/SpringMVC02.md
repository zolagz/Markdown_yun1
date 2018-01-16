# SpringMVC02


###窄化请求影响路径

在class上添加@RequestMapping(url)指定通用请求前缀， 限制此类下的所有方法请
求url必须以请求前缀开头，通过此方法对url进行分类管理。如下：

```@RequestMapping放在类名上边，设置请求前缀 @Controller@RequestMapping("/item")```方法名上边设置请求映射url
```
@RequestMapping放在方法名上边，如下：@RequestMapping("/queryItem ")```
访问地址为：/item/queryItem

###请求方法限定 *	限定GET方法
>@RequestMapping(method = RequestMethod.GET)>>
>>如果通过Post访问则报错：>>
>>HTTP Status 405 - Request method 'POST' not supported例如：```
@RequestMapping(value="/editItem",method=RequestMethod.GET)
```*	限定POST方法>@RequestMapping(method = RequestMethod.POST)>>如果通过Post访问则报错：>>>>HTTP Status 405 - Request method 'GET' not supported*	GET和POST都可以>@RequestMapping(method={RequestMethod.GET,RequestMethod.POST})###controller方法返回值

*  返回ModelAndView	controller方法中定义ModelAndView对象并返回，对象中可添加model数据、指定view。* 返回void在controller方法形参上可以定义request和response，使用request或response指定响应结果：>1、使用request转向页面，如下：
>request.getRequestDispatcher("页面路径").forward(request, response);>2、也可以通过response页面重定向：>response.sendRedirect("url")>3、也可以通过response指定响应结果，例如响应json数据如下：
>
```response.setCharacterEncoding("utf-8");response.setContentType("application/json;charset=utf-8");response.getWriter().write("json串");* 返回字符串逻辑视图名controller方法返回字符串可以指定逻辑视图名，通过视图解析器解析为物理视图地址。//指定逻辑视图名，经过视图解析器解析为jsp物理路径：/WEB-INF/jsp/item/editItem.jspreturn "item/editItem";* 请求重定向与请求转发的区别：请求转发forward：浏览器URL地址不会发生变化，在服务器端做了一个跳转，通一个request域中请求重定向redirect：浏览器地址会发生变化，服务器端向浏览器响应302状态码，浏览器收到302之后，重新请求服务器一个新的地址，不同的request域了* Redirect重定向实现商品修改完成之后跳转到商品列表页面![](http://p2ehgqigv.bkt.clouddn.com/18-1-16/94198586.jpg)

Contrller方法返回结果重定向到一个url地址，如下商品修改提交后重定向到商品查询方法，参数无法带到商品查询方法中。```
//重定向到queryItem.action地址,request无法带过去return "redirect:queryItem.action";

```redirect方式相当于“response.sendRedirect()”，转发后浏览器的地址栏变为转发后的地址，因为转发即执行了一个新的request和response。由于新发起一个request原来的参数在转发时就不能传递到下一个url，如果要传参数可以/item/queryItem.action后边加参数，如下：/item/queryItem?...&…..* forward转发
实现商品修改完成之后，通过请求转发，继续跳转到修改页面controller方法执行后继续执行另一个controller方法，如下商品修改提交后转向到商品修改页面，修改商品的id参数可以带到商品修改方法中。![](http://p2ehgqigv.bkt.clouddn.com/18-1-16/24278932.jpg)
```
//结果转发到editItem.action，request可以带过去return "forward:editItem.action";
```forward方式相当于“request.getRequestDispatcher().forward(request,response)”，转发后浏览器地址栏还是原来的地址。转发并没有执行新的request和response，而是和转发前的请求共用一个request和response。所以转发前请求的参数在转发后仍然可以读取到。


<!--
create time: 2018-01-16 19:49:07
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

