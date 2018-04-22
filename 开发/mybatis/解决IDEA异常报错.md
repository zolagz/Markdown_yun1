# 解决IDEA异常报错

###解决org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)错误
[来源scdn](https://blog.csdn.net/oMrLeft123/article/details/70239951)

错误提示：

```
　org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)　
```

一般的原因
Mapper interface和xml文件的定义对应不上，需要检查包名，namespace，函数名称等能否对应上。
按以下步骤一一执行：<p>
1、检查xml文件所在的package名称是否和interface对应的package名称一一对应<p>
2、检查xml文件的namespace是否和xml文件的package名称一一对应<p>
3、检查函数名称能否对应上<p>
4、去掉xml文件中的中文注释<p>
5、随意在xml文件中加一个空格或者空行然后保存<p>
注意：<p>
在使用IDEA开发时，如果打包时*Mapper.xml没有自动复制到class输出目录的mapper类包下，则需要在pom文件中添加mybatis加载配置文件的配置！
如下：

```
<build>
　　<resources>
　　　　<resource>
　　　　　　　　<directory>src/main/java</directory>
　　　　　　<includes>
　　　　　　　　<include>**/*.xml</include>
　　　　　　</includes>
　　　　</resource>
　　　　<resource>
　　　　　　<directory>src/main/resources</directory>
　　　　</resource>
　　</resources>
</build>

```

该问题就是IDEA下碰到的，在pom文件中添加mybatis加载配置文件然后就完美解决了




<!--
create time: 2018-04-19 12:08:35
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

