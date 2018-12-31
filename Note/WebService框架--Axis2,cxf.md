# WebService框架--Axis2,cxf

> Web service是一个平台独立的，松耦合的、自包含的、基于可编程的web的应用程序，可使用开放的XML标准来描述、发布、发现、协调和配置这些应用程序，用于开发分布式的互操作的应用程序。
>
> 通过使用WebService，我们能够像调用本地方法一样去调用远程服务器上的方法。我们并不需要关心远程的那个方法是Java写的，还是PHP或C#写的；我们并不需要关心远程的方法是基于Unix平台，还是Windows平台，也就是说WebService与平台和语言无关。


关于WebService是什么可以参考阮一峰老师的文章[Web service是什么？](http://www.ruanyifeng.com/blog/2009/08/what_is_web_service.html)

这里简单介绍两个写WebService接口时所使用的框架Axis2和cxf

## Axis2

一般用Axis2写客户端比较简单,如下面的例子。只需要配好maven依赖，然后直接写Java类即可。

```Java
public class WebserviceClient {

    public static void main(String[] args) {

        try {
            //本机tomcat端口为8080,参数是wsdl网址的一部分
            EndpointReference targetEPR = new EndpointReference("http://localhost:8080/services/ZipKinService");
            RPCServiceClient sender = new RPCServiceClient();
            Options options = sender.getOptions();
            //超时时间20s
            options.setTimeOutInMilliSeconds(2 * 20000L);
            options.setTo(targetEPR);
            /**
             * 参数:
             * 1：在网页上执行 wsdl后xs:schema标签的targetNamespace路径
             * <xs:schema  targetNamespace="http://axis2.com">
             * 2：<xs:element name="test"> ======这个标签中name的值
             */
            QName qname = new QName("http://axis2.com", "test");
            //方法的入参
            String str = "我是客户端";
            Object[] param = new Object[]{str};
            //这是针对返值类型的
            Class<?>[] types = new Class[]{String.class};
            /**
             * RPCServiceClient类的invokeBlocking方法调用了WebService中的方法。
             * invokeBlocking方法有三个参数
             * 第一个参数的类型是QName对象，表示要调用的方法名；
             * 第二个参数表示要调用的WebService方法的参数值，参数类型为Object[]；
             * 第三个参数表示WebService方法的返回值类型的Class对象，参数类型为Class[]。
             * 当方法没有参数时，invokeBlocking方法的第二个参数值不能是null，而要使用new Object[]{}。
             */
            Object[] response = sender.invokeBlocking(qname, param, types);
            System.out.println(response[0]);
        } catch (AxisFault e) {
            e.printStackTrace();
        }
    }
}
```

## CXF

CXF 的话可以参考这篇文章中[WebService就是这么简单](https://juejin.im/post/5aadae4bf265da238a303917) CXF 的部分。
