##sacla安装


1.下载 http://www.scala-lang.org/download/

下载后进入安装包所在目录进行解压操作


2.配置环境变量

Mac修改 .bash_profile 文件，此文件是mac 当前用户的环境配置文件。

/etc/profile 是当前系统的环境配置文件（Linux，系统可修改这个）

.bash_profile 文件的路径是在当前用户下。

```
vi .bash_profile
//添加如下信息
export SCALA_HOME=你Scala的路径/scala
export PATH=$PATH:$SCALA_HOME/bin

```

3.检验结果

在终端输入scala 命令，进入scala解释器，然后输入1＋2，查看计算结果。

如下图说明安装成功： 

![](http://p2ehgqigv.bkt.clouddn.com/18-2-25/12338847.jpg)

4.退出

```
输入
 :q   
    或
 :quit 
```


