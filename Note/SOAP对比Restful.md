# SOAP 对比 Restful

在学习过程及个人项目中一直在使用Restful的方式，也很喜欢Restful的简洁。但在实习的过程中，使用SOAP格式来搭建webservice服务，也了解到了一些SOAP的优点。

## webservice

Webservice的一个最基本的目的就是提供在各个不同平台的不同应用系统的协同工作能力。

Web service 就是一个应用程序，它向外界暴露出一个能够通过Web进行调用的API。

## SOAP

SOAP(简单对象访问协议)是一种数据交换协议规范，是一种轻量的、简单的、基于XML的协议的规范。

优点为： 易用，灵活，跨语言，跨平台。

* 易用：是因为它的消息是基于xml并封装成了符合http协议，因此,它符合任何路由器、 防火墙或代理服务器的要求。

* 灵活：表现在极具拓展性，SOAP 无需中断已有的应用程序, SOAP 客户端、 服务器和协议自身都能发展。而且SOAP 能极好地支持中间介质和层次化的体系结构。

* 跨语言：soap可以使用任何语言来完成，只要发送正确的soap请求即可。

* 跨平台：基于soap的服务可以在任何平台无需修改即可正常使用。

SOAP请求是HTTP POST的一个专用版本，遵循一种特殊的xml消息格式Content-type设置为: text/xml任何数据都可以xml化。

SOAP在安全方面是通过使用XML-Security和XML-Signature两个规范组成了WS-Security来实现安全控制的，当前已经得到了各个厂商的支持，.net ，php ，java 都已经对其有了很好的支持 。这是REST薄弱的地方。

## Restful


Rest主要是基于http之上建立的一种接口规范，核心是资源。因此REST对于资源型服务接口来说很合适，同时特别适合对于效率要求很高，但是对于安全要求不高的场景。

其他笔记有记录Restful的内容，这里就不赘述了。

---

参考内容

[Service Station - More On REST](https://msdn.microsoft.com/en-us/magazine/dd942839.aspx)

[WebService的两种方式SOAP和REST比较](https://www.jianshu.com/p/1d091b693a2c)

[wiki--SOAP](https://en.wikipedia.org/wiki/SOAP)
