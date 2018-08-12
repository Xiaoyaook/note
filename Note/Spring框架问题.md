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
