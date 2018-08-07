# 安装Zookeeper


安装步骤：
第一步：安装 jdk第二步：下载 zookeeper 的压缩包 [百度网盘](https://pan.baidu.com/s/1o9mA03K)第三步：解压缩压缩包	tar -zxvf zookeeper-3.4.6.tar.gz第四步：进入 zookeeper-3.4.6 目录，创建 data 文件夹。	mkdir data第五步：进入conf目录 ，把 zoo_sample.cfg 改名为 zoo.cfg	cd conf	mv zoo_sample.cfg zoo.cfg第六步：打开zoo.cfg ,  修改 data 属性：dataDir=/root/zookeeper-3.4.6/
data3.2.3 Zookeeper 服务启动进入bin目录，启动服务输入命令 	./zkServer.sh start输出以下内容表示启动成功关闭服务输入命令	./zkServer.sh stop输出以下提示信息查看状态：	./zkServer.sh status如果启动状态，提示![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/4862651.jpg)如果未启动状态，提示：
![](http://p2ehgqigv.bkt.clouddn.com/18-1-14/88545790.jpg)

<!--
create time: 2018-01-14 16:03:08
Author: Alfred

This file is created by Marboo<http://marboo.io> template file $MARBOO_HOME/.media/starts/default.md
本文件由 Marboo<http://marboo.io> 模板文件 $MARBOO_HOME/.media/starts/default.md 创建
-->

