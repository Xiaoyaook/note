# Spring Web MVC

## 1.1. Introduction

Spring Web MVC 是基于 Servlet API 的 web框架.

Spring Framework 5.0引入了基于 reactive stack 的web框架Spring WebFlux.

## 1.2. DispatcherServlet

和所有 Servlet 一样，DispatcherServlet 需要被符合Servlet规范的Java配置文件或 web.xml 来声明和映射。

DispatcherServlet 通过使用 Spring配置 来找到其进行request mapping, view resolution, exception handling所需的组件。

两个例子：通过Java配置或web.xml配置 来进行注册和初始化 DispatcherServlet。

### 1.2.1. Context Hierarchy

DispatcherServlet 需要一个 WebApplicationContext 来进行构造。这也是其源码中的构造函数所需要的唯一一个参数。

当一个 WebApplicationContext 被多个 DispatcherServlet 实例共享时，我们也许需要一个上下文层级关系。

两个例子：通过Java配置或web.xml 来配置 WebApplicationContext 层级。

### 1.2.2. Special Bean Types

DispatcherServlet 委托一些特殊的 Bean 来进行请求的处理和返回适当的回应。

列举一些能被 DispatcherHandler 检查到的 Special Bean

**HandlerMapping**，**HandlerAdapter**，HandlerExceptionResolver，**ViewResolver**，LocaleResolver, LocaleContextResolver，
ThemeResolver，MultipartResolver，FlashMapManager

### 1.2.3. Web MVC Config

在大多数情况下，MVC Config 是最佳起点。

### 1.2.4. Servlet Config

在 Servlet3.0+ 的环境下，我们可以选择以编程方式配置Servlet容器。

一个例子，以编程方式注册 DispatcherServlet。

实现了 WebApplicationInitializer 接口的抽象类 AbstractDispatcherServletInitializer 使我们更简单的注册 DispatcherServlet，我们只需要重写方法来指定 Servlet 的映射，以及定位 DispatcherServlet 的位置。

两个例子，分别基于Java和xml的 Spring配置，继承 AbstractDispatcherServletInitializer。

AbstractDispatcherServletInitializer 还提供了一个方便的方法，来添加 Filter 实例，并自动映射到 DispatcherServlet。

AbstractDispatcherServletInitializer 的 isAsyncSupported 方法，对 DispatcherServlet 提供了异步的支持。

### 1.2.5. Processing

DispatcherServlet 按照以下流程处理请求：Deferred

[The DispatcherServlet processes requests as follows](https://docs.spring.io/spring/docs/5.0.8.RELEASE/spring-framework-reference/web.html#mvc-servlet-sequence)

我们可以通过将Servlet初始化参数（init-param元素）添加到web.xml文件中的Servlet声明，来自定义独立的 DispatcherServlet 实例。

给一个 DispatcherServlet 支持的初始化参数列表。

### 1.2.6. Interception

拦截器必须实现 HandlerInterceptor 接口，其中提供三个方法来对请求进行预处理和后期处理。

注意 postHandle 方法，对于@ResponseBody和ResponseEntity方法不太有用，因为 response 在 postHandle 之前写入和提交。
但也不是所有情况都不适用，比如想要在response中添加一个header那就不适用，而只是要对访问的ip，操作之类的做一下记录的话还是适用的。

### 1.2.7. Exceptions

在请求映射和请求处理的过程中抛出了异常，DispatcherServlet 委托给一个 HandlerExceptionResolver 链，来解析并处理异常。

给一个 HandlerExceptionResolver 的实现列表。

### 1.2.8. View Resolution

Spring MVC定义了 ViewResolver 和 View 接口，使我们能够在浏览器中渲染模型，而无需与特定的视图技术联系起来。

ViewResolver 提供视图名称和实际视图之间的映射。

View 在移交给特定视图技术之前解决数据准备问题。

给一个 ViewResolver 层级关系表。

#### Handling,Redirecting,Forwarding,Content negotiation

### 1.2.9. Locale

### 1.2.10. Themes

### 1.2.11. Multipart resolver

## 1.3. Filters

spring-web 模块提供了一些有用的过滤器。

## 1.4. Annotated Controllers

Spring MVC提供了一个基于注解的编程模型，其中@Controller和@RestController组件使用注解来表达请求映射，请求输入，异常处理等等。

这一部分没什么好说的，具体看文档。

## 1.5. URI Links

构造URI

## 1.6. Async Requests

异步请求

## 1.7. CORS

### 1.7.1. Introduction

出于安全原因，浏览器禁止对当前源外的资源进行AJAX调用。

Cross-Origin Resource Sharing (CORS) 是一个W3C标准，被众多浏览器所实现。允许我们指定授权哪种类型的跨域请求，而不是使用基于IFRAME或JSONP的安全性较低且功能较弱的解决方法。

### 1.7.2. Processing

CORS规范，把请求分为三种：预检，简单和实际请求。

要了解CORS是如何工作的 可以看MDN上这篇文章 [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

为了实现跨域请求，我们需要一些显式声明的CORS配置。如果未找到匹配的CORS配置，将会拒绝预检请求。

### 1.7.3. @CrossOrigin

@CrossOrigin 注解允许对带有注解的controller进行跨域请求。

@CrossOrigin 的默认配置为：

* All origins.
* All headers.
* All HTTP methods to which the controller method is mapped.
* allowedCredentials is not enabled by default since that establishes a trust level that exposes sensitive user-specific information such as cookies and CSRF tokens, and should only be used where appropriate.
* maxAge is set to 30 minutes.

@CrossOrigin 也支持对类注解，类的所有方法将会继承该注解。

### 1.7.4. Global Config

两个例子：Java和xml 配置 对CORS进行全局配置。

### 1.7.5. CORS Filter

我们还可以通过内置的 CorsFilter 来支持 CORS。

> 如果你想用 CorsFilter 配合 Spring Security，注意 Spring Security 内置了对 CORS 的支持。

要配置这个filter，我们需要把 CorsConfigurationSource 传递给其构造函数。

给一个配置的例子。

## 1.8. Web Security

参考[Spring Security](https://docs.spring.io/spring-security/site/docs/5.0.7.RELEASE/reference/htmlsingle/)文档

## 1.9. HTTP Caching

这部分介绍Spring Web MVC中可用的HTTP缓存相关选项

## 1.10. View Technologies

视图技术，主要介绍一些模板引擎的整合。
