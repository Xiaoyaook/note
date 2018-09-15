# SpringBoot2.x + Actuator 监控

## Actuator 的作用

Spring Boot Actuator 可以帮助我们在应用程序部署在生产环境后，**监控我们的应用程序，收集指标，了解流量或数据库的状态**。

我们可以通过选择使用HTTP或JMX端点（endpoints）来与 Actuator 交互。

## 引入依赖

首先我们引入依赖，注意本文使用的Spring Boot基于当前最新版的 2.0.4.RELEASE.

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

## 内置Endpoints

与1.x版本不同，Actuator 2.x 禁用了大多数端点。

**默认情况**下，只有 / health和 / info 是**可用的**。

我们可以通过配置`management.endpoints.web.exposure.include = *`将所有端点设置为可用。

下面是一些常用的端点:

* **health** ：显示应用的健康信息
* **info** ：显示任意的应用信息
* **metrics** ：显示当前应用的 metrics 信息
* **httptrace** ：显示HTTP跟踪信息（默认显示最后100个HTTP请求 - 响应交换）

完整内置端点列表和详情可见 [官方文档 Endpoints](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints)

## 创建定制端点

```Java
@Component
@Endpoint(id = "features")
public class FeaturesEndpoint  {
    private Map<String, Feature> features = new ConcurrentHashMap<>();

    @ReadOperation
    public Map<String, Feature> features() {
        return features;
    }

    @ReadOperation
    public Feature feature(@Selector String name) {
        return features.get(name);
    }

    @WriteOperation
    public void configureFeature(@Selector String name, Feature feature) {
        features.put(name, feature);
    }

    @DeleteOperation
    public void deleteFeature(@Selector String name) {
        features.remove(name);
    }

    public static class Feature {
        private Boolean enabled;

        public Boolean getEnabled() {
            return enabled;
        }

        public void setEnabled(Boolean enabled) {
            this.enabled = enabled;
        }
    }
}
```

我们的端点的路径由`@Endpoint`的 id 参数决定，在我们的例子中，它将请求路由到 / actuator / features。

## 扩展现有端点

我们可以使用`@EndpointExtension`注解,或其更具体指定的注解`@EndpointWebExtension`或`@EndpointJmxExtension`轻松扩展预定义端点的行为

```Java
@Component
@EndpointWebExtension(endpoint = InfoEndpoint.class)
public class InfoWebEndpointExtension {
    private InfoEndpoint delegate;

    // standard constructor

    @ReadOperation
    public WebEndpointResponse<Map> info() {
        Map<String, Object> info = this.delegate.info();
        Integer status = getStatus(info);
        return new WebEndpointResponse<>(info, status);
    }

    private Integer getStatus(Map<String, Object> info) {
        // return 5xx if this is a snapshot
        return 200;
    }
}
```

## 开启所有端点

添加以下配置以公开所有端点:

`management.endpoints.web.exposure.include=*`

要显式启用特定端点（例如/ shutdown），我们添加以下配置：

`management.endpoint.shutdown.enabled=true`

显示所有健康状态：

`management.endpoint.health.show-details = always`

要关闭一个特定端点（例如/ loggers），我们添加以下配置：

`management.endpoints.web.exposure.exclude=loggers`

## 图像化表示端点数据

Spring Boot Admin

需要分别创建服务端和客户端，然后从客户端访问服务端

[Spring Boot Admin Reference Guide](http://codecentric.github.io/spring-boot-admin/2.0.2/)

---

推荐阅读：

[Spring Boot 2.0官方文档之 Actuator](https://blog.csdn.net/alinyua/article/details/80009435) Actuator中文文档

[Spring Boot Actuator: Production-ready features](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready) 官方文档

[Spring Boot Actuator](https://www.baeldung.com/spring-boot-actuators) 本文代码示例来自于此