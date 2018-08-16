# IoC container

IoC（控制反转）也被称为dependency injection (依赖注入)。

这是一个对象定义它们的依赖关系的过程，也就是说，它们只与构造函数参数、对工厂方法的参数、或在对象实例上设置的属性或从工厂方法返回的属性有关。
然后容器在创建bean时注入这些依赖项。
这个过程基本是逆向的，故称为IoC（控制反转）。

bean本身通过直接构造类来控制实例化或者定位其依赖。

org.springframework.beans 和 org.springframework.context 包是构成Spring框架IoC容器的基础。

[BeanFactory](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/javadoc-api/org/springframework/beans/factory/BeanFactory.html) 接口提供能够管理任何类型对象的高级配置机制。
[ApplicationContext](https://docs.spring.io/spring-framework/docs/5.0.8.RELEASE/javadoc-api/org/springframework/context/ApplicationContext.html) 是 BeanFactory 的子接口，他和其他的Spring特性更容易集成。

简单来说，BeanFactory 提供配置框架和基本功能，ApplicationContext 是他的超集。

在Spring中，构成应用程序主干并由Spring IOC容器管理的对象称为bean。

bean是一个实例，它由Spring IOC容器实例化、组装和管理。

BeanDefinition-> 元数据

Instantiating beans

1.4.1 Dependency resolution process 解决依赖的过程

The Spring container validates the configuration of each bean as the container is created. 

However, the bean properties themselves are not set until the bean is actually created.

Beans that are singleton-scoped and set to be pre-instantiated (the default) are created when the container is created.  早期引用，解决循环依赖问题（在Circular dependencies中有解释）

1.5 Bean scopes 
bean作用域

singleton scope

Spring’s concept of a singleton bean **differs** from the Singleton pattern as defined in the Gang of Four (GoF) patterns book.

The GoF Singleton hard-codes the scope of an object such that one and only one instance of a particular class is created per ClassLoader.

The scope of the Spring singleton is best described as per container and per bean. This means that if you define one bean for a particular class in a single Spring container, then the Spring container creates one and only one instance of the class defined by that bean definition.

prototype scope

 As a rule, use the prototype scope for all stateful beans and the singleton scope for stateless beans.

 1.6  Customizing the nature of a bean
 bean 生命周期

Combining lifecycle mechanisms

 三种控制bean生命周期的方式：
 InitializingBean and DisposableBean 回调接口
 init() and destroy() 方法
 @PostConstruct and @PreDestroy annotations注解

 三种方式组合使用的各种情况及执行顺序 

 Lifecycle 接口 LifecycleProcessor 接口 

 SmartLifecycle 接口

 Container Extension Points 容器扩展点

 我们无需直接继承ApplicationContext的实现类，Spring IoC 容器可以通过实现一些特定的接口来扩展

 BeanPostProcessor 接口
 
 If you want to implement some custom logic after the Spring container finishes instantiating, configuring, and initializing a bean, you can plug in one or more BeanPostProcessor implementations.

 1.9 基于注解的配置

 



