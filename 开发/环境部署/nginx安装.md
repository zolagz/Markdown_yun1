（1）需要安装 gcc 的环境【此步省略】

```yum install gcc-c++```（2）第三方的开发包。【此步省略】 PCRE   PCRE(Perl Compatible Regular Expressions)是一个 Perl 库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库。```yum install -y pcre pcre-devel```注：pcre-devel 是使用 pcre 开发的一个二次开发库。nginx 也需要此库。## zlibzlib 库提供了很多种压缩和解压缩的方式，nginx 使用 zlib 对 http 包的内容进行 gzip，所以需要在 linux 上安装 zlib 库。```
yum install -y zlib zlib-devel```
## OpenSSLOpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。nginx 不仅支持 http 协议，还支持 https（即在 ssl 协议上传输 http），所以需要在 linux安装 openssl 库。```yum install -y openssl openssl-devel```
###2.2.2 Nginx下载官方网站下载 nginx：http://nginx.org/我们课程中使用的版本是 1.8.0 版本。###2.2.3 Nginx安装第一步：把 nginx 的源码包nginx-1.8.0.tar.gz上传到 linux 系统Alt+p  启动sftp  ,将nginx-1.8.0.tar.gz上传第二步：解压缩	tar zxvf nginx-1.8.0.tar.gz第三步：进入nginx-1.8.0目录   使用 configure 命令创建一 makeFile 文件。```./configure \--prefix=/usr/local/nginx \--pid-path=/var/run/nginx/nginx.pid \--lock-path=/var/lock/nginx.lock \--error-log-path=/var/log/nginx/error.log \--http-log-path=/var/log/nginx/access.log \--with-http_gzip_static_module \--http-client-body-temp-path=/var/temp/nginx/client \--http-proxy-temp-path=/var/temp/nginx/proxy \--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \--http-scgi-temp-path=/var/temp/nginx/scgi```执行后可以看到Makefile文件
![](http://p2ehgqigv.bkt.clouddn.com/18-1-22/78675313.jpg)

第四步：编译make第五步：安装make install

如果遇到如下问题，可以忽略，[参考链接](https://www.fujieace.com/nginx/make1-leaving-directory-usrlocalnginx.html)

![](http://p2ehgqigv.bkt.clouddn.com/18-1-22/30802759.jpg)

![](http://p2ehgqigv.bkt.clouddn.com/18-1-22/30305728.jpg)


3 Nginx启动与访问注意：启动nginx 之前，上边将临时文件目录指定为/var/temp/nginx/client， 需要在/var  下创建此 目录```
mkdir /var/temp/nginx/client -p```
进入到Nginx目录下的sbin目录```
cd /usr/local/ngiux/sbin```
输入命令启动Nginx

```./nginx```

启动后查看进程

```ps aux|grep nginx```
![](http://p2ehgqigv.bkt.clouddn.com/18-1-22/33219990.jpg)

![](http://p2ehgqigv.bkt.clouddn.com/18-1-22/87437997.jpg)


关闭 nginx：

```
./nginx -s stop
```
或者

```
./nginx -s quit

```
重启 nginx：	

```
1、先关闭后启动。
2、刷新配置文件：
./nginx -s reload
```