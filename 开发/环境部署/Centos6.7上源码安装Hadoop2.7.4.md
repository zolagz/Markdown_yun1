# Centos6.7上源码安装Hadoop2.7.4


###一、	准备的资料

在这里提供一个已经在centos6.7上编译好的hadoop2.7.4的安装包，[编译好的hadoop2.7.4](https://pan.baidu.com/s/1c3cLQxi)，安装如下步骤，配置好环境，直接解压本包即可使用；
源码根目录下有个BUILDINT.txt，打开即可看见里面关于编译hadoop的一些环境要求　　64位linux系统CentOS 6.7。
JDK 1.7+。
maven-3.2.5。 一个项目管理综合工具, 使用标准的目录结构和默认构建生命周期protobuf 2.5.0  google的一种数据交换的格式，它独立于语言，独立于平台hadoop-2.x.x-src 　　 ant-1.9.7　　将软件编译、测试、部署等步骤联系在一起加以自动化的一个工具　
这些文件，需要上传到linux系统中，我使用的是SSH来进行上传的，直接上传到了 /root 目录下。同时由于安装过程中需要在线下载东西，古需要保持linux系统的网络畅通。###二、安装JDK

[jdk8下载](https://pan.baidu.com/s/1nwHKZ3r)
[linux安装jdk](https://www.jianshu.com/p/81347430dffb)###三、安装maven
1 下载maven 3.2.1

2 下载完成后，解压到某个目录这个和安装jdk是一样的，安装成后同样需要配置环境变量
![](http://p2ehgqigv.bkt.clouddn.com/18-2-1/63776057.jpg)同样，输入命令使配置文件生效　　
	source /etc/profile    
检测是否安装成功：　　	mvn  -version能够看到关于maven的一些信息，包括所使用的jdk

![](http://p2ehgqigv.bkt.clouddn.com/18-2-1/18129211.jpg)###四、安装protobuf 
[protobuf下载](https://pan.baidu.com/s/1nwbN6CL)这一步比较关键，protobuf最好是提前下载然后上传上去。安装protobuf前，需要安装一些其他的东西

```
  yum  install  gcc    安装c++ 
  yum  install  gcc-c++          然后会两次提示输入 y（yes/no） 
  yum install  make          可能会提示因为make已经是最新版本，而		不会安装，这个无所谓，反正是最新版本，就不安装了
```接下来便是安装protobuf，同样是一个解压缩的过程
```
cd  /roottar -zxvf protobuf-2.5.0.tar.gz    然后进入到安装目录中，以此输入一下命令：　  cd  /protobuf-2.5.0  ./ configure  make  make  install
```   我在进行上面第二个命令，对configure进行预编译时报错，查看异常后，是c++没有安装成功，便再一次执行了：yum install  gcc-c++ 命令，OK   make  命令执行的时间比较长，最后一个命令的执行安装，如果出现错误，重新来过即可   测试    protoc  --version ###五、安装CMake　   CMake 需要2.6以上的版本，在安装的时候会提示输入 y/N，同时由于安装这个组件是需要联网的，故根据网速不同，安装的进度也不一样```
     yum  install  cmake          yum  install  openssl-devel
``` ###六、ant 安装   
[ant下载](https://pan.baidu.com/s/1sm2JK53)	
	tar zxvf  apache-ant-1.9.4-bin.tar.gz  
配置环境变量```
 export ANT_HOME=/root/apache-ant-1.9.4 export PATH=.:$PATH:$JAVA_HOME/bin:$MAVEN_HOME/bin:$ANT_HOME/bin
```生效以及测试　　

```
	source /etc/profile    ant  -version
```###七、编译hadoop 
[hadoop下载](https://pan.baidu.com/s/1brg1RWV)1、解压hadoop源码包　　	tar -zxvf hadoop-2.5.2-src.tar.gz
 进入文件夹里面，里面有一个文件BUILDINT.txt，打开即可看见里面关于编译hadoop的一些环境要求　　
2、编译hadoop	进入解压后的原文件中，然后输入一下命令即可 



``` 
cd /root/hadoop-2.5.2-src/
mvn package -Pdist,native -DskipTests -Dtar        或者是下面的命令　mvn  package -DeskipTests -Pdist,native

```查看版本号：

```
[root@node2 hadoop-2.7.4]# cd bin
[root@node2 bin]# hadoop version
Hadoop 2.7.4
Subversion Unknown -r Unknown
Compiled by root on 2017-08-23T13:31Z
Compiled with protoc 2.5.0
From source with checksum 50b0468318b4ce9bd24dc467b7ce1148
This command was run using /usr/local/dev/hadoop-2.7.4/share/hadoop/common/hadoop-common-2.7.4.jar

```3、等带编译结果编译完成后会有提示，SUCCESS / FAILURE。如下　　　如果编译失败，向上滚动，变能够查看什么地方出错了。4、查看编译结果　[编译后的压缩包](https://pan.baidu.com/s/1c3cLQxi)

同样在刚刚进行编译的那个目录下，有一个 hadoop-dist文件夹，进入里面的target文

件夹，然后就可以看到编译成功64位的hadoop文件，解压后的在 hadoop-2.5.2 这个文

件夹中，同时还生成了一个压缩包：hadoop-2.7.4-tar.gz 这个压缩包可以拷贝到别的

机器上进行安装 参考链接：[Centos上源码安装Hadoop2.7.2](http://blog.csdn.net/libingxin/article/details/51097071)

<!--
create time: 2018-02-01 21:17:40
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

