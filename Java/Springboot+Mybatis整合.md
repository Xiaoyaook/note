# Springboot + Mybatis整合

使用xml进行配置

## pom 添加 Mybatis 依赖
```
<!-- Spring Boot Mybatis 依赖 -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>${mybatis-spring-boot}</version>
</dependency>
<!-- MySQL 连接驱动依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql-connector}</version>
        </dependency>
```

## application.properties 应用配置文件，增加 Mybatis 相关配置
```
## Mybatis 配置
mybatis.typeAliasesPackage=org.spring.springboot.domain
mybatis.mapperLocations=classpath:mapper/*.xml
```
其他参数可参考 官方文档[Configuration](http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)部分.

## 在 Application 应用启动类添加注解 MapperScan
Application.java 代码如下：
```
@SpringBootApplication
// mapper 接口类扫描包配置
@MapperScan("org.spring.springboot.dao")
public class Application {
 
    public static void main(String[] args) {
        // 程序启动入口
        // 启动嵌入式的 Tomcat 并初始化 Spring 环境及其各 Spring 组件
        SpringApplication.run(Application.class,args);
    }
}
```