### SpringMVC + Spring + MyBatis 整合

[手把手教你整合最优雅SSM框架：SpringMVC + Spring + MyBatis](http://blog.csdn.net/qq598535550/article/details/51703190)大体来自此博文,加入一些个人对配置的理解与修改

首先用maven创建项目,建立项目的目录结构,我用的是IDEA,使用其内置的maven进行创建.

在main文件夹下创建一个java文件夹,并设置为Sources Root,作为我们的根地址.

接下来就在Sources Root下创建几个Package:

* dao : 数据访问层（接口）
* entity : 实体类
* dto : 数据传输层
* service : 业务逻辑（接口）
* serviceImpl : 业务逻辑（实现）
* controller : 控制器

接下来导入所需要的jar包,由于使用了maven来管理我们的jar,所以只需要在`pom.xml`中加入相应的依赖即可.

####　pom.xml
在这里注意,jar包的版本很重要,有时会因为版本出现一些错误,之前在整合spring和mybatis在执行数据库操作的时候报出了：
`java.lang.AbstractMethodError:org.mybatis.spring.transaction.SpringManagedTransaction.getTimeout()L`就是版本兼容出现了问题,把pom中mybatis-spring版本升级为1.3.0解决了此问题.
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>xiaoyaook</groupId>
  <artifactId>MyBatis_Study</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>MyBatis_Study Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
    <!-- 注意maven自带的junit版本太低,升级一下 -->
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>

    <!-- 1.日志 -->
    <!-- 实现slf4j接口并整合 -->
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.1.1</version>
    </dependency>

    <!-- 2.数据库 -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>6.0.6</version>
    </dependency>
    <dependency>
      <groupId>com.mchange</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.5.2</version>
    </dependency>

    <!-- DAO: MyBatis -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.5</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
    </dependency>

    <!-- 3.Servlet web -->
    <dependency>
      <groupId>taglibs</groupId>
      <artifactId>standard</artifactId>
      <version>1.1.2</version>
    </dependency>
    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.5.4</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    </dependency>

    <!-- 4.Spring -->
    <!-- 1)Spring核心 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>4.1.7.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>4.1.7.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>4.1.7.RELEASE</version>
    </dependency>
    <!-- 2)Spring DAO层 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>4.1.7.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>4.1.7.RELEASE</version>
    </dependency>
    <!-- 3)Spring web -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>4.1.7.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>4.1.7.RELEASE</version>
    </dependency>
    <!-- 4)Spring test -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>4.1.7.RELEASE</version>
    </dependency>

    <!-- redis客户端:Jedis -->
    <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
      <version>2.9.0</version>
    </dependency>
    <dependency>
      <groupId>com.dyuproject.protostuff</groupId>
      <artifactId>protostuff-core</artifactId>
      <version>1.1.2</version>
    </dependency>
    <dependency>
      <groupId>com.dyuproject.protostuff</groupId>
      <artifactId>protostuff-runtime</artifactId>
      <version>1.1.3</version>
    </dependency>

    <!-- Map工具类 -->
    <dependency>
      <groupId>commons-collections</groupId>
      <artifactId>commons-collections</artifactId>
      <version>3.2</version>
    </dependency>
  </dependencies>

  <build>
    <finalName>MyBatis_Study</finalName>
  </build>
</project>
```
#### 接下来进行Spring 和 Mybatis的配置
在项目目录下的resources文件夹中创建两个文件夹`mapper`和`spring`.
spring的配置太多，我们这里分三层，分别是dao service web.

先在spring文件夹里新建`spring-dao.xml`文件
1. 读入数据库连接相关参数（可选）
1. 配置数据连接池
1. 配置连接属性，可以不读配置项文件直接在这里写死
1. 配置c3p0，只配了几个常用的
1. 配置SqlSessionFactory对象（mybatis）
1. 扫描dao层接口，动态实现dao接口，也就是说不需要daoImpl，sql和参数都写在xml文件上

##### spring-dao.xml
*注意配置的时候我们使用了一个classpath来索引一些文件的位置,这个classpath代表的就是编译后的生成的target文件夹中的classes的位置*
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 配置整合mybatis过程 -->
    <!-- 1.配置数据库相关参数properties的属性：${url} -->
    <context:property-placeholder location="classpath:jdbc.properties" />

    <!-- 2.数据库连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!-- 配置连接池属性 -->
        <property name="driverClass" value="${jdbc.driver}" />
        <property name="jdbcUrl" value="${jdbc.url}" />
        <property name="user" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />

        <!-- c3p0连接池的私有属性 -->
        <property name="maxPoolSize" value="30" />
        <property name="minPoolSize" value="10" />
        <!-- 关闭连接后不自动commit -->
        <property name="autoCommitOnClose" value="false" />
        <!-- 获取连接超时时间 -->
        <property name="checkoutTimeout" value="10000" />
        <!-- 当获取连接失败重试次数 -->
        <property name="acquireRetryAttempts" value="2" />
    </bean>

    <!-- 3.配置SqlSessionFactory对象 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource" />
        <!-- 配置MyBaties全局配置文件:mybatis-config.xml -->
        <property name="configLocation" value="classpath:mybatis-config.xml" />
        <!-- 扫描entity包 使用别名 -->
        <property name="typeAliasesPackage" value="integr.entity" />
        <!-- 扫描sql配置文件:mapper需要的xml文件 -->
        <property name="mapperLocations" value="classpath:mapper/*.xml" />
    </bean>

    <!-- 4.配置扫描Dao接口包，动态实现Dao接口，注入到spring容器中 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 注入sqlSessionFactory -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <!-- 给出需要扫描Dao接口包 -->
        <property name="basePackage" value="integr.dao" />
    </bean>
</beans>
```
因为数据库配置相关参数是读取配置文件，所以在resources文件夹里新建一个jdbc.properties文件，存放我们4个最常见的数据库连接属性
##### jdbc.properties
*这里我用的系统是Ubuntu16.04,直接用root账户进行连接的话,会被拒绝连接,不清楚是什么原因,但新建一个MySQL账户并赋予权限的话就可以连接得上.*
```
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=utf8
jdbc.username= root
jdbc.password=
```
因为这里用到了mybatis，所以需要配置mybatis核心文件，在recources文件夹里新建mybatis-config.xml文件

1. 使用自增主键
2. 使用列别名
3. 开启驼峰命名转换 create_time -> createTime

##### mybatis-config.xml
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 配置全局属性 -->
    <settings>
        <!-- 使用jdbc的getGeneratedKeys获取数据库自增主键值 -->
        <setting name="useGeneratedKeys" value="true" />

        <!-- 使用列别名替换列名 默认:true -->
        <setting name="useColumnLabel" value="true" />

        <!-- 开启驼峰命名转换:Table{create_time} -> Entity{createTime} -->
        <setting name="mapUnderscoreToCamelCase" value="true" />
    </settings>

</configuration>
```
弄好dao层，接下来到service层了。在spring文件夹里新建`spring-service.xml`文件
1. 扫描service包所有注解 @Service
1. 配置事务管理器，把事务管理交由spring来完成
1. 配置基于注解的声明式事务，可以直接在方法上@Transaction

##### spring-service.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd">
    <!-- 扫描service包下所有使用注解的类型 -->
    <context:component-scan base-package="integr.service" />

    <!-- 配置事务管理器 -->
    <bean id="transactionManager"
          class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 注入数据库连接池 -->
        <property name="dataSource" ref="dataSource" />
    </bean>

    <!-- 配置基于注解的声明式事务 -->
    <tx:annotation-driven transaction-manager="transactionManager" />
</beans>
```
配置web层，在spring文件夹里新建spring-web.xml文件
1. 开启SpringMVC注解模式，可以使用@RequestMapping，@PathVariable，@ResponseBody等
1. 对静态资源处理，如js，css，jpg等
1. 配置jsp 显示ViewResolver，例如在controller中某个方法返回一个string类型的"login"，实际上会返回"/WEB-INF/login.jsp"
1. 扫描web层 @Controller

##### spring-web.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/mvc
	http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">
    <!-- 配置SpringMVC -->
    <!-- 1.开启SpringMVC注解模式 -->
    <!-- 简化配置：
        (1)自动注册DefaultAnootationHandlerMapping,AnotationMethodHandlerAdapter
        (2)提供一些列：数据绑定，数字和日期的format @NumberFormat, @DateTimeFormat, xml,json默认读写支持
    -->
    <mvc:annotation-driven />

    <!-- 2.静态资源默认servlet配置
        (1)加入对静态资源的处理：js,gif,png
        (2)允许使用"/"做整体映射
     -->
    <mvc:default-servlet-handler/>

    <!-- 3.配置jsp 显示ViewResolver -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 4.扫描web相关的bean -->
    <context:component-scan base-package="integr.web" />
</beans>
```
这里作者用的是jsp,如果想要用thymleaf的话,把配置jsp那部分去掉,加上下面这部分
```
    <bean id="templateResolver"
         class="org.thymeleaf.spring4.templateresolver.SpringResourceTemplateResolver">
     <property name="prefix" value="/WEB-INF/pages/" />
     <property name="suffix" value=".html" />
     <property name="templateMode" value="HTML" />
     <property name="cacheable" value="false" />
   </bean>

   <bean id="templateEngine"
         class="org.thymeleaf.spring4.SpringTemplateEngine">
     <property name="templateResolver" ref="templateResolver" />
   </bean>

   <bean class="org.thymeleaf.spring4.view.ThymeleafViewResolver">
     <property name="templateEngine" ref="templateEngine" />
   </bean>
```
最后就是修改web.xml文件了，它在webapp的WEB-INF下
##### web.xml
```
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1" metadata-complete="true">
  <!-- 如果是用mvn命令生成的xml，需要修改servlet版本为3.1 -->
  <!-- 配置DispatcherServlet -->
  <servlet>
    <servlet-name>seckill-dispatcher</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 配置springMVC需要加载的配置文件
        spring-dao.xml,spring-service.xml,spring-web.xml
        Mybatis - > spring -> springmvc
     -->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:spring/spring-*.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>seckill-dispatcher</servlet-name>
    <!-- 默认匹配所有的请求 -->
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```
我们在项目中经常会使用到日志，所以这里还有配置日志xml，在resources文件夹里新建logback.xml文件，所给出的日志输出格式也是最基本的控制台s呼出，大家有兴趣查看logback官方文档
##### logback.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
	<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
		<!-- encoders are by default assigned the type ch.qos.logback.classic.encoder.PatternLayoutEncoder -->
		<encoder>
			<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
		</encoder>
	</appender>

	<root level="debug">
		<appender-ref ref="STDOUT" />
	</root>
</configuration>
```