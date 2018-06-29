# Nginx

Nginx是一款开源的HTTP服务器软件

HTTP服务器本质上也是一种应用程序——它通常运行在服务器之上，**绑定服务器的IP地址并监听某一个tcp端口来接收并处理HTTP请求**，这样客户端（一般来说是IE, Firefox，Chrome这样的浏览器）就能够通过HTTP协议来获取服务器上的网页（HTML格式）、文档（PDF格式）、音频（MP4格式）、视频（MOV格式）等等资源

## 静态HTTP服务器

将服务器上的静态文件（如HTML、图片）通过HTTP协议展现给客户端

极简配置：

```
server {
	listen 80; # 端口号
	location / {
		root /usr/share/nginx/html; # 静态文件路径
	}
}
```

## 反向代理

客户端本来可以直接通过HTTP协议访问某网站应用服务器，如果网站管理员在中间加上一个Nginx，客户端请求Nginx，Nginx请求应用服务器，然后将结果返回给客户端，此时Nginx就是反向代理服务器。

负载均衡、虚拟主机，都基于反向代理实现，当然反向代理的功能也不仅仅是这些。

极简配置：
```
server {
	listen 80;
	location / {
		proxy_pass http://192.168.20.1:8080; # 应用服务器HTTP地址
	}
}
```

## 负载均衡

将相同的应用部署在多台服务器上，将大量用户的请求分配给多台机器处理。带来的好处是，其中一台服务器万一挂了，只要还有其他服务器正常运行，就不会影响用户使用。

极简配置:
```
upstream myapp {
	server 192.168.20.1:8080; # 应用服务器1
	server 192.168.20.2:8080; # 应用服务器2
}
server {命令
	listen 80;
	location / {
		proxy_pass http://myapp;
	}
}
```


## 虚拟主机

将多个网站部署在同一台服务器上。

例如将www.aaa.com和www.bbb.com两个网站部署在同一台服务器上，两个域名解析到同一个IP地址，但是用户通过两个域名却可以打开两个完全不同的网站，互相不影响，就像访问两个服务器一样，所以叫两个虚拟主机。

极简配置:

```命令
server {
	listen 80 default_server;
	server_name _;
	return 444; # 过滤其他域名的请求，返回444状态码
}
server {
	listen 80;
	server_name www.aaa.com; # www.aaa.com域名
	location / {
		proxy_pass http://localhost:8080; # 对应端口号8080
	}
}
server {
	listen 80;
	server_name www.bbb.com; # www.bbb.com域名
	location / {
		proxy_pass http://localhost:8081; # 对应端口号8081
	}
}
```

在服务器8080和8081分别开了一个应用，客户端通过不同的域名访问，根据server_name可以反向代理到对应的应用服务器。

虚拟主机的原理是通过HTTP请求头中的Host是否匹配server_name来实现的，有兴趣的同学可以研究一下HTTP协议。

另外，server_name配置还可以过滤有人恶意将某些域名指向你的主机服务器。

# Tomcat

Nginx本身不支持生成动态页面，但它们可以通过其他模块来支持（例如通过Shell、PHP、Python脚本程序来动态生成内容）。

如果想要使用Java程序来动态生成资源内容，使用这一类HTTP服务器很难做到。Java Servlet技术以及衍生的Java Server Pages技术可以让Java程序也具有处理HTTP请求并且返回内容（由程序动态控制）的能力，Tomcat正是支持运行Servlet/JSP应用程序的容器（Container），称为应用服务器（Application Server）。为了方便，应用服务器往往也会集成 HTTP Server 的功能，但是不如专业的 HTTP Server 那么强大。

Tomcat运行在JVM之上，它和HTTP服务器一样，绑定IP地址并监听TCP端口，同时还包含以下指责：

* 管理Servlet程序的生命周期
* 将URL映射到指定的Servlet进行处理
* 与Servlet程序合作处理HTTP请求——根据HTTP请求生成HttpServletResponse对象并传递给Servlet进行处理，将Servlet中的HttpServletResponse对象生成的内容返回给浏览器

虽然Tomcat也可以认为是HTTP服务器，但通常它仍然会和Nginx配合在一起使用：
* 动静态资源分离——运用Nginx的反向代理功能分发请求：所有动态资源的请求交给Tomcat，而静态资源的请求（例如图片、视频、CSS、JavaScript文件等）则直接由Nginx返回到浏览器，这样能大大减轻Tomcat的压力。
* 负载均衡，当业务压力增大时，可能一个Tomcat的实例不足以处理，那么这时可以启动多个Tomcat实例进行水平扩展，而Nginx的负载均衡功能可以把请求通过算法分发到各个不同的实例进行处理

# 实际部署问题

Nginx安装与配置文件详解可参考[nginx.conf配置文件](https://segmentfault.com/a/1190000002797601#articleHeader5)

Nginx配置文件主要分成四部分：main（全局设置）、server（主机设置）、upstream（上游服务器设置，主要为反向代理、负载均衡相关配置）和 location（URL匹配特定位置后的设置）。

对于location部分的编写是尤为基础和关键的，可以参考[nginx配置location总结及rewrite规则写法](https://segmentfault.com/a/1190000002797606)

## Vue部署

在我们的项目是vue-cli搭建的条件下，

1. 使用`npm run build`打包出来dist目录
2. 把dist里的文件打包上传到服务器 例 /data/www/，我一般把index.html放在static里，所以我的文件路径为：
``` 
/data/www/static    
|-----index.html   
|-----js    
|-----css    
|-----images    
....
```
3. 配置nginx监听80端口， location /static alias 到 /data/www/static，重启nginx
```
location /static {
alias /data/www/static/;
}
```
4. 浏览器访问http://ip/static/index.html即可

## SpringBoot部署

在我们的项目是基于Maven构建的条件下,

1. 用mvn clean package将springboot后端项目打包，会在target目录下出现一个****.jar文件(取决与我们的项目名称与版本)
2. 进入target目录，用java -jar ****.jar运行后端项目，因为springboot是默认继承了web容器的，所以不用另外安装tomcat了
3. 改下nginx的配置文件了，在nginx.conf文件中的server下新增
```
location /api/ 
{
    proxy_pass http://192.168.1.19:9090/;
} 
```
4. 转发到后端接口时不要写http://localhost，一定要写http://ip地址，不然会有莫名其妙的错误，而且很难发现

---

部分引用自：
[知乎](https://www.zhihu.com/question/32212996/answer/87524617)
[知乎](https://www.zhihu.com/question/46630687/answer/102284339)
[如何在本地部署vue+springboot前后端分离应用](https://blog.csdn.net/u012755393/article/details/78823066)
[Nginx基本功能极速入门](http://xxgblog.com/2015/05/17/nginx-start/)