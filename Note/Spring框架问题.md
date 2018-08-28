# Spring框架问题

## ioc里面的循环依赖是如何解决的

重点在于“早期引用”，提前暴露对象，来解决

DefaultSingletonBeanRegistry类里有一组缓存：
* singletonObjects : 用于存放完全初始化好的 bean，从该缓存中取出的 bean 可以直接使用
* earlySingletonObjects : 存放原始的 bean 对象（尚未填充属性），用于解决循环依赖
* singletonFactories : 存放 bean 工厂对象，用于解决循环依赖

[Spring IOC 容器源码分析 - 循环依赖的解决办法](https://cloud.tencent.com/developer/article/1145275)

## spring MVC的工作流程是怎样的

主要在于 DispatcherServlet 类， 其中的initStrategies方法，实现了String MVC的初始化工作。

1. 用户发送请求至前端控制器DispatcherServlet。
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器。
3. 处理器映射器找到具体的处理器，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。
4. DispatcherServlet调用HandlerAdapter处理器适配器。
5. HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。
6. Controller执行完成返回ModelAndView。
7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器。
9. ViewReslover解析后返回具体View。
10. DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。
11. DispatcherServlet响应用户。

## 什么是AOP？Spring中为啥要引入AOP或者AOP有什么好处？

AOP简单了说就是在目标方法执行前可以自定义一些操作，在方法执行中或者执行后也可以自定义操作，所以，一般都是基于代理模式来实现，Spring支持3种代理模式，jdk原生的代理和cglib代理,AspectJ静态代理。

AOP可以给程序带良好的扩展性和封装性，可以实现业务代码与非业务代码的隔离。比如：可以在不改变目标代码的前提下，实现目标方法的增强，比如做方法执行时间监控，记录方法访问日志，再比如：数据库的connection.close()默认是把连接关闭掉，但是数据库连接池的场景中，为了不改变用户的使用习惯，一般调用close的时候是把连接重新放回到池中，这是因为从数据库连接池中拿到的连接实际上是原生连接的一个代理类，所以内部把close给重写了。实际上代理模式的优点实际上也是AOP的优点。

## spring boot 缺点优点？

Spring Boot 的优点快速开发，特别适合构建微服务系统，另外给我们封装了各种经常使用的套件，比如mybatis、hibernate、redis、mongodb等。

使用 Spring 项目引导页面可以在几秒构建一个项目方便对外输出各种形式的服务，如 REST API、WebSocket、Web、Streaming、Tasks非常简洁的安全策略集成支持关系数据库和非关系数据库支持运行期内嵌容器，如 Tomcat、Jetty强大的开发包，支持热启动自动管理依赖自带应用监控支持各种 IDE，如 IntelliJ IDEA 、NetBeans

缺点是集成度较高，使用过程中不太容易了解底层。