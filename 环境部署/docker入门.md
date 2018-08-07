###Docker入门

####引言（centos7.x环境下安装docker）

Docker官方建议在Ubuntu中安装，因为Docker是基于Ubuntu发布的，而且一般Docker出现的问题Ubuntu是最先更新或者打补丁的。在很多版本的CentOS中是不支持更新最新的一些补丁包的。
由于我们学习的环境都使用的是CentOS，因此这里我们将Docker安装到CentOS上。注意：这里**建议安装在CentOS7.x以上**的版本，在CentOS6.x的版本中，安装前需要安装其他很多的环境而且Docker很多补丁不支持更新

安装：(centos7.0下)

```
yum install docker
```
查看安装的Docker版本

	docker -v

启动与停止docker

```
	启动docker：systemctl start docker	停止docker：systemctl stop docker	重启docker：systemctl restart docker	查看docker状态：systemctl status docker	开机启动：systemctl enable docker
	查看docker概要信息：docker info	查看docker帮助文档：docker --help
```

列出镜像

>docker images

![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/81538614.jpg)

	REPOSITORY：镜像所在的仓库名称	TAG：镜像标签	IMAGE ID：镜像ID	CREATED：镜像的创建日期（不是获取该镜像的日期）	SIZE：镜像大小这些镜像都是存储在Docker宿主机的/var/lib/docker目录下

###搜索镜像
如果你需要从网络中查找需要的镜像，可以通过以下命令搜索	
	docker search 镜像名称![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/21053481.jpg)

	NAME：仓库名称	DESCRIPTION：镜像描述	STARS：用户评价，反应一个镜像的受欢迎程度	OFFICIAL：是否官方	AUTOMATED：自动构建，表示该镜像由Docker Hub自动构建流程创建的###拉取镜像

使用命令拉取：>docker pull centos:7目前国内访问docker hub速度上有点尴尬，使用docker Mirror势在必行。现有国内提供docker镜像加速服务的商家有不少，下面重点ustc镜像。

####ustc的镜像ustc是老牌的linux镜像服务提供者了，还在遥远的ubuntu 5.04版本的时候就在

用。ustc的docker镜像加速器速度很快。ustc docker mirror的优势之一就是

不需要注册，是真正的公共服务。https://lug.ustc.edu.cn/wiki/mirrors/help/docker步骤：（1）编辑该文件：vi /etc/docker/daemon.json  // 如果该文件不存在就手动创建；说明：在centos7.x下，通过vi。

![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/77534096.jpg)

(2)在该文件中输入如下内容：

```
{"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]}

```（3）注意：一定要重启docker服务，如果重启docker后无法加速，可以重新启动OS

![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/5370820.jpg)

然后通过docker pull命令下载镜像：速度杠杠的

###删除镜像1、	docker rmi $IMAGE_ID：删除指定镜像2、	docker rmi `docker images -q`：删除所有镜像

![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/88554744.jpg)

###Docker容器操作查看容器查看正在运行容器：	docker ps	
查看所有的容器（启动过的历史容器）：	docker ps –a![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/87339480.jpg)查看最后一次运行的容器：	docker ps –l![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/26442581.jpg)

查看停止的容器	docker ps -f status=exited	
	
###创建与启动容器
创建容器常用的参数说明：	创建容器命令：docker run	-i：表示运行容器	-t：表示容器启动后会进入其命令行。加入这两个参数后，容器创建就能登录进去。即分配一个伪终端。	--name :为创建的容器命名。	-v：表示目录映射关系（前者是宿主机目录，后者是映射到宿主机上的目录），可以使用多个－v做多个目录或文件映射。注意：最好做目录映射，在宿主机上做修改，然后共享到容器上。	-d：在run后面加上-d参数,则会创建一个守护式容器在后台运行（这样创建容器后不会自动登录容器，如果只加-i -t两个参数，创建后就会自动进去容器）。	-p：表示端口映射，前者是宿主机端口，后者是容器内的映射端口。可以使用多个－p做多个端口映射###交互式容器创建一个交互式容器并取名为mycentos>docker run -it --name=mycentos centos:7 /bin/bash这时我们通过ps命令查看，发现可以看到启动的容器，状态为启动状态![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/45139970.jpg)
使用exit命令 退出当前容器![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/57275419.jpg)
然后用ps -a 命令查看发现该容器也随之停止：![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/13679836.jpg)

###守护式容器创建一个守护式容器：如果对于一个需要长期运行的容器来说，我们可以创建一个守护式容器。命令如下（容器名称不能重复）：>docker run -di --name=mycentos2 centos:7####	登录守护式容器方式：
docker exec -it container_name (或者 container_id)  /bin/bash（exit退出时，容器不会停止）
![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/52743544.jpg)
###停止与启动容器停止正在运行的容器：

>docker stop $CONTAINER_NAME/ID![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/10271115.jpg)

启动已运行过的容器：

>docker start $CONTAINER_NAME/ID

![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/13449552.jpg)

###文件拷贝如果我们需要将文件拷贝到容器内可以使用cp命令>docker cp 需要拷贝的文件或目录 容器名称:容器目录也可以将文件从容器内拷贝出来>docker cp 容器名称:容器目录 需要拷贝的文件或目录###目录挂载
我们可以在创建容器的时候，将宿主机的目录与容器内的目录进行映射，这样我们就可以通过修改宿主机某个目录的文件从而去影响容器。创建容器 添加-v参数 后边为   宿主机目录:容器目录>docker run -di -v /usr/local/myhtml:/usr/local/myhtml --name=mycentos2 centos:7如果你共享的是多级的目录，可能会出现权限不足的提示。这是因为CentOS7中的安全模块selinux把权限禁掉了，我们需要添加参数  --privileged=true  来解决挂载的目录没有权限的问题![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/21292424.jpg)###查看容器IP地址我们可以通过以下命令查看容器运行的各种数据>docker inspect mycentos2也可以直接执行下面的命令直接输出IP地址>docker inspect --format='{{.NetworkSettings.IPAddress}}' mycentos2###删除容器删除指定的容器：docker rm $CONTAINER_ID/NAME
![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/30504131.jpg)
注意，只能删除停止的容器
删除所有容器：
>docker rm `docker ps -a -q`
![](http://p2ehgqigv.bkt.clouddn.com/18-1-26/81652465.jpg)