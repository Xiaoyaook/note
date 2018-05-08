# HikariCP是什么?

HikariCP是一个高性能的JDBC连接池。Hikari是日语“光”的意思。可能是目前java业界最快的数据库连接池（BoneCP因此停止维护，其作者推荐HikariCP).

## 数据库连接池技术

数据库连接池负责分配、管理和释放数据库的连接。

* 数据库连接复用。重复使用现有的数据库连接，可以避免连接频繁建立、关闭的开销。
* 统一的连接管理。释放空闲时间超过最大空闲时间的数据库连接，避免因为没有释放数据库连接而引起的数据库连接泄漏。

## HikariCP是如何实现这么高效率

* 字节码精简：优化代码，直到编译后的字节码最少，这样，CPU缓存可以加载更多的程序代码；
* 优化代理和拦截器：减少代码，例如HikariCP的Statement proxy只有100行代码，只有BoneCP的十分之一；
* 自定义数组类型（FastStatementList）代替ArrayList：避免每次get()调用都要进行range check，避免调用remove()时的从头到尾的扫描；
* 自定义集合类型（ConcurrentBag）：提高并发读写的效率；
* 其他针对BoneCP缺陷的优化

### Maven依赖(Java8/9)
```
<dependency>
        <groupId>com.zaxxer</groupId>
        <artifactId>HikariCP</artifactId>
        <version>3.1.0</version>
    </dependency>
```

### Spring配置文件
```
<!--2.数据库连接池-->
    <bean id="dataSource" class="com.zaxxer.hikari.HikariDataSource">
        <!--配置连接池属性-->
        <property name="driverClassName" value="${driver}" />

        <!-- 基本属性 url、user、password -->
        <property name="jdbcUrl" value="${url}" />
        <property name="username" value="${username}" />
        <property name="password" value="${password}" />

        <!-- 连接只读数据库时配置为true， 保证安全 -->
        <property name="readOnly" value="false" />

        <!-- 连接池中允许的最大连接数。缺省值：10；推荐的公式：((core_count * 2) + effective_spindle_count) -->
        <property name="maximumPoolSize" value="60" />
        <property name="minimumIdle" value="10" />
        <!-- 等待连接池分配连接的最大时长（毫秒），超过这个时长还没可用的连接则发生SQLException， 缺省:30秒 -->
        <property name="connectionTimeout" value="30000" />
        <!-- 一个连接idle状态的最大时长（毫秒），超时则被释放（retired），缺省:10分钟 -->
        <property name="idleTimeout" value="600000" />
        <!-- 一个连接的生命时长（毫秒），超时而且没被使用则被释放（retired），缺省:30分钟，建议设置比数据库超时时长少30秒，参考MySQL
            wait_timeout参数（show variables like '%timeout%';） -->
        <property name="maxLifetime" value="1800000" />
    </bean>
```

[其他参数参考github官方网址](https://github.com/brettwooldridge/HikariCP)