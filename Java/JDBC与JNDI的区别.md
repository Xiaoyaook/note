# JDBC 与 JNDI 的区别

实习工作时，公司在开发环境使用 JDBC 的方式连接数据库，而在生产环境使用 JNDI 的方式连接数据库。下面介绍一下这两种方式的异同。

## Java Database Connectivity API

（Java Database Connectivity，简称JDBC）是Java语言中用来规范客户端程序如何来访问数据库的应用程序接口，提供了诸如查询和更新数据库中数据的方法。

JDBC API主要位于JDK中的java.sql包中（之后扩展的内容位于javax.sql包中），主要包括（斜体代表接口，需驱动程序提供者来具体实现）：

* DriverManager：负责加载各种不同驱动程序（Driver），并根据不同的请求，向调用者返回相应的数据库连接（Connection）。
* Driver：驱动程序，会将自身加载到DriverManager中去，并处理相应的请求并返回相应的数据库连接（Connection）。
* Connection：数据库连接，负责进行与数据库间的通讯，SQL执行以及事务处理都是在某个特定Connection环境中进行的。可以产生用以执行SQL的Statement。
* Statement：用以执行SQL查询和更新（针对静态SQL语句和单次执行）。
* PreparedStatement：用以执行包含动态参数的SQL查询和更新（在服务器端编译，允许重复执行以提高效率）。
* CallableStatement：用以调用数据库中的存储过程。
* SQLException：代表在数据库连接的创建和关闭和SQL语句的执行过程中发生了例外情况（即错误）。

## Java Naming and Directory Interface

Java命名和目录接口（Java Naming and Directory Interface，缩写JNDI），是Java的一个目录服务应用程序界面（API），它提供一个目录系统，并将服务名称与对象关联起来，从而使得开发人员在开发过程中可以使用名称来访问对象。

例如我们通过指定一个资源名称，该名称对应于数据库或命名服务中的一个记录，则返回数据库连接建立所必须的信息。

JNDI主要有两部分组成：应用程序编程接口和服务供应商接口。应用程序编程接口提供了Java应用程序访问各种命名和目录服务的功能，服务供应商接口提供了任意一种服务的供应商使用的功能。

如果应用程序在Web容器内运行，则通常使用JNDI来允许在Web容器中配置和管理连接池，而不是在应用程序内部进行配置和管理。

---

参考资料：

[wiki--JDBC](https://zh.wikipedia.org/wiki/Java%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%9E%E6%8E%A5)

[stackoverflow--Differences of connection pool, jdbc and jndi](https://stackoverflow.com/questions/15676990/differences-of-connection-pool-jdbc-and-jndi)

[(ORACLE)Connecting Pool--DataSource](https://docs.oracle.com/javase/tutorial/jdbc/basics/sqldatasources.html)

[(ORACLE)JDBC](https://docs.oracle.com/javase/tutorial/jdbc/)

[(ORACLE)JNDI](https://docs.oracle.com/javaee/6/tutorial/doc/bncji.html)
