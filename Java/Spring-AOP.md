# Spring-AOP

## 10.1 Introduction

AOP与OOP的不同点

AOP框架是Spring的重要组成部分，但Spring IOC容器并不依赖于AOP。

AOP在Spring框架中的主要应用：
* 提供声明式服务，如声明式事务管理
* 允许用户实现定制的切面

### 10.1.1 AOP concepts

介绍一些AOP关键的概念和术语，以及通知的类型.

### 10.1.2 Spring AOP capabilities and goals

Spring AOP是由Pure Java实现，所以不需要特别的编译处理。

Spring AOP与其他AOP框架的不同。

### 10.1.3 AOP Proxies

Spring AOP 默认使用JDK动态代理进行AOP代理。

Spring AOP 还支持CGLIB代理。

CGLIB主要用于未实现接口的类的代理。

## 10.2 @AspectJ support

Spring AOP 无缝支持AspectJ.

@AspectJ 方式，注解

### 10.2.1 Enabling @AspectJ Support

开启@AspectJ支持，通过 Java configuration 或者 XML configuration。

### 10.2.2 Declaring an aspect 

通过　@Aspect　注解声明一个切面

### 10.2.3 Declaring a pointcut

声明一个切点，很长的介绍及实例，没有细看

### 10.2.4 Declaring advice

声明式通知，与切点配合使用．很长的介绍及实例，没有细看

### 10.2.5 Introductions

### 10.2.6 Aspect instantiation models

### 10.2.7 Example

## 10.3 Schema-based AOP support

XML形式的切面定义与使用，不感兴趣，过．．．

## 10.4 Choosing which AOP declaration style to use

选择哪种AOP的声明风格

Spring AOP or AspectJ

Aspect language (code) style, @AspectJ annotation style, or the Spring XML style

取决于项目需要，开发工具，以及团队对AOP的熟悉程度。

### 10.4.1 Spring AOP or full AspectJ?

如果我们只需要对Spring Bean对象进行通知，Spring AOP是个更好的选择。

### 10.4.2 @AspectJ or XML for Spring AOP?

我选注解风格！

### 10.5 Mixing aspect types

混合风格～

## 10.6 Proxying mechanisms

代理机制

默认JDK动态代理，如果目标对象没有实现任何接口，则选择CGLIB代理

使用CGLIB代理，有几点问题需要注意：
* 因为final不能被重写，所以final 方法不能被通知
* 还有两个与Spring版本有关的改进，不用管了

### 10.6.1 Understanding AOP proxies

理解AOP代理模式

先介绍了普通对象引用的方法调用，然后是通过代理对象的方法调用.

其实就是设计模式中的代理模式

## 10.7 Programmatic creation of @AspectJ Proxies

利用AspectJProxyFactory自动对 @AspectJ aspect进行代理。

## 10.8 Using AspectJ with Spring applications

使用AspectJ代替Spring AOP，过。。。

---
[10. Aspect Oriented Programming with Spring](https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/aop.html)