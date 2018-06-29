# Rustful架构与前后端分离

## 什么是RESTful架构?

1. 每一个URI代表一种资源
2. 客户端和服务器之间，传递这种资源的某种表现层；
3. 客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"

[理解RESTful架构](http://www.ruanyifeng.com/blog/2011/09/restful.html)

## RESTful架构的优点

简单来说：
1. 看Url就知道要什么
2. 看http method就知道干什么
3. 看http status code就知道结果如何

[RESTful的特征和优点](https://www.cnblogs.com/imyalost/p/7923230.html)

## 前后端分离

### 方案一

前端使用MVVM框架编写SPA(Single Page Application)应用，后端服务器只提供RESTful接口且完全无状态化。

优点：

* 前后完全分离，前端开发者完全不需要关心服务端用了什么技术，只需要一份接口说明文档即可。
* 手机App与PC端网站可以共用同一套接口。

缺点：

* 如果”页面”过多，SPA应用第一次加载速度会稍慢。
* SEO困难。

### 方案二

使用NodeJS渲染Web页面，然后调用后端RESTful接口。

优点：

* 前后完全分离, 后端开发者可以专注于业务逻辑开发。

缺点：

* 代码如果异常处理不好容易直接挂掉进程。
* 增加了部署和维护成本。
* 增加了一层NodeJS，提高了网络传输的开销。
