# CAS--单点登录

cas服务端其实就是一个war包

在资源\cas\source\cas-server-4.0.0-release\cas-server-4.0.0\modules目录下cas-server-webapp-4.0.0.war  将其改名为cas.war放入tomcat目录下的webapps
下。启动tomcat自动解压war包。浏览器输入

http://localhost:8080/cas/login 可看到登录页面

![](http://p2ehgqigv.bkt.clouddn.com/18-1-13/44173800.jpg)不要嫌弃这个页面丑，我们后期可以再提升它的颜值。暂时把注意力放在功能实现上。这里有个固定的用户名和密码   casuser /Mellon登录成功后会跳到登录成功的提示页面
![](http://p2ehgqigv.bkt.clouddn.com/18-1-13/26652345.jpg)1.4 CAS服务端配置1.4.1端口修改如果我们不希望用8080端口访问CAS, 可以修改端口修改TOMCAT的端口打开tomcat 目录 conf\server.xml  找到下面的配置添加默认用户名和密码

编辑cas的WEB-INF/deployerConfigContext.xml文件


```
    <bean id="primaryAuthenticationHandler"
          class="org.jasig.cas.authentication.AcceptUsersAuthenticationHandler">
        <property name="users">
            <map>
                <entry key="casuser" value="Mellon"/>
                <entry key="admin" value="admin"/>
            </map>
        </property>
    </bean>

```

将端口8080，改为9100
![](http://p2ehgqigv.bkt.clouddn.com/18-1-13/75876986.jpg)修改CAS配置文件修改cas的WEB-INF/cas.properties ```
server.name=http://localhost:9100```

####修改退出后跳转![](http://p2ehgqigv.bkt.clouddn.com/18-1-13/48453090.jpg)```
	<bean id="logoutAction" class="org.jasig.cas.web.flow.LogoutAction"
		p:servicesManager-ref="servicesManager"
		p:followServiceRedirects="${cas.logout.followServiceRedirects:true}" /> 
```

###1.4.2去除https认证
CAS默认使用的是HTTPS协议，如果使用HTTPS协议需要SSL安全证书（需向特定的机构申请和

购买） 。如果对安全要求不高或是在开发测试阶段，可使用HTTP协议。我们这里讲解通过修改

配置，让CAS使用HTTP协议。修改cas的WEB-INF/deployerConfigContext.xml找到下面的配置```<bean class="org.jasig.cas.authentication.handler.support.HttpBasedServiceCredentialsAuthenticationHandler"p:httpClient-ref="httpClient"/>```
这里需要增加参数p:requireSecure="false"，requireSecure属性意思为是否需要安全

验证，即HTTPS，false为不采用

```
<bean class="org.jasig.cas.authentication.handler.support.HttpBasedServiceCredentialsAuthenticationHandler"p:httpClient-ref="httpClient" p:requireSecure="false"/>

```修改cas的/WEB-INF/spring-configuration/ticketGrantingTicketCookieGenerator.xml
####找到下面配置```
<bean id="ticketGrantingTicketCookieGenerator" class="org.jasig.cas.web.support.CookieRetrievingCookieGenerator"      p:cookieSecure="true"      p:cookieMaxAge="-1"      p:cookieName="CASTGC"      p:cookiePath="/cas" />
      ```
参数p:cookieSecure="true"，同理为HTTPS验证相关，TRUE为采用HTTPS验证，FALSE为不采用https验证。参数p:cookieMaxAge="-1"，是COOKIE的最大生命周期，-1为无生命周期，即只在当前打开的窗口有效，关闭或重新打开其它窗口，仍会要求验证。可以根据需要修改为大于0的数字，比如3600等，意思是在3600秒内，打开任意窗口，都不需要验证。我们这里将cookieSecure改为false ,  cookieMaxAge 改为3600（3）修改cas的WEB-INF/spring-configuration/warnCookieGenerator.xml找到下面配置```
<bean id="warnCookieGenerator" class="org.jasig.cas.web.support.CookieRetrievingCookieGenerator"p:cookieSecure="true"p:cookieMaxAge="-1"p:cookieName="CASPRIVACY"p:cookiePath="/cas" />```
我们这里将cookieSecure改为false ,  cookieMaxAge 改为3600


<!--
create time: 2018-01-18 11:27:36
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

