ActiveMQ下载与安装

###第一步：下载
官方网站下载：http://activemq.apache.org/将apache-activemq-5.12.0-bin.tar.gz 上传至服务器
###解压此文件tar  zxvf  apache-activemq-5.12.0-bin.tar.gz为apache-activemq-5.12.0目录赋权chmod 777 apache-activemq-5.12.0进入apache-activemq-5.12.0\bin目录###赋与执行权限chmod 755 activemq 
> linux 命令chmod 755的意思
> >chmod是Linux下设置文件权限的命令，后面的数字表示不同用户或用户组的权限。
>>一般是三个数字：
>>第一个数字表示文件所有者的权限>
>第二个数字表示与文件所有者同属一个用户组的其他用户的权限>
>第三个数字表示其它用户组的权限。  
>权限分为三种：读（r=4），写（w=2），执行（x=1） 。 

####启动 ./activemq start出现下列提示表示成功！
![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/63100875.jpg)
假设服务器地址为192.168.25.135 ，打开浏览器输入地址http://192.168.25.135:8161/ 即可进入ActiveMQ管理页面
![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/90999428.jpg)点击进入管理页面![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/84052878.jpg)
输入用户名和密码  均为 admin ![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/42405204.jpg)

进入主页面
![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/66725935.jpg)

点对点消息列表
![](http://p2ehgqigv.bkt.clouddn.com/18-1-12/59896580.jpg)

列表各列信息含义如下：
Number Of Pending Messages  ：等待消费的消息 这个是当前未出队列的数量。Number Of Consumers  ：消费者 这个是消费者端的消费者数量Messages Enqueued  ：进入队列的消息  进入队列的总数量,包括出队列的。Messages Dequeued  ：出了队列的消息  可以理解为是消费这消费掉的数量。
