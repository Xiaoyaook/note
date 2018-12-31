# JDBC 与 Mybatis 与 Hibernate

## JDBC

### 一次JDBC连接实例

```Java
import java.sql.*;

/**
 * JDBC案例类.
 *
 */
public class JDBC {
    public static void main(String[] args) {
        // 定义连接对象
        Connection connection = null;
        // 定义预编译语句对象
        PreparedStatement preparedStatement = null;
        // 定义结果集对象
        ResultSet resultSet = null;
        try {
            // 加载数据库驱动
            Class.forName("com.mysql.jdbc.Driver");
            // 通过数据库驱动获取数据库连接
            connection = DriverManager.
                    getConnection("jdbc:mysql://localhost:3306/jdbc?characterEncoding=utf-8", "root", "root");
            // 定义并初始化SQL语句
            String sql = "select * from user where id = ?";
            // 创建预编译语句对象
            preparedStatement = connection.prepareStatement(sql);
            // 设置查询参数
            preparedStatement.setString(1, "1");
            // 向数据库发出SQL并执行查询,获取查询结果集.
            resultSet = preparedStatement.executeQuery();
            // 遍历查询结果集
            while (resultSet.next()) {
                // 输出查询结果
                System.out.println("id = " + resultSet.getString("id"));
                System.out.println("name = " + resultSet.getString("name"));
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally { // 释放资源
            if (resultSet != null) {
                try {
                    // 关闭结果集对象
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

            if (preparedStatement != null) {
                try {
                    // 关闭预编译语句对象
                    preparedStatement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

            if (connection != null) {
                try {
                    // 关闭连接对象
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### JDBC存在问题分析

* 频繁地开启和关闭数据库连接，严重地影响了数据库的性能。

* 代码中存在硬编码。每当需求变更时，都需要修改源代码然后重新编译，系统不易维护。硬编码的具体体现：

  * SQL语句中的查询条件(String sql = "select * from user where id = ?";)。如果需求变更，改为根据用户主键和用户名进行查询，那么就需要修改源代码。
  * 设置查询参数时的参数位置和参数值(preparedStatement.setString(1, "1");)。如果SQL语句由于需求变更而发生变化，那么原来编写的赋值语句就可能出错，需要修改源代码。
  * 获取查询结果时指定的列名(resultSet.getString("name");)。如果数据表的字段名发生了变化，那么就需要修改源代码。
* 设置查询参数的过程繁琐。因为在设置查询参数的过程中，需要为查询语句中不同位置上的占位符设置不同的值。
* 获取查询结果的过程繁琐。因为在获取查询结果的过程中，需要遍历查询结果集，而且在遍历的过程中还需要根据数据表的列名才可以获取出其值。


## Mybatis

MyBatis是一个基于Java的、封装了JDBC的持久层框架。

### 为什么选择MyBatis

> MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架。MyBatis 避免了几乎所有的 JDBC
代码和手工设置参数以及抽取结果集。MyBatis 使用简单的 XML 或注解来配置和映射基本体，将接口和 Java的
POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。（选自官网）

也就是说开发者只需要关注SQL本身，而不需要花费精力去处理例如注册驱动、创建connection、创建statement、手动设置参数、结果集检索等jdbc的过程代码。MyBatis通过xml或注解的方式将要执行的各种statement（statement，preparedStatement、CallableStatement）配置起来，并通过Java对象和statement中的SQL进行映射生成最终执行的SQL语句，最后又mybatis框架执行SQL并将结果映射成Java对象并返回。

### MyBatis的运行流程

在SQLMapConfig.xml(mybati的全局配置文件)中配置mybatis的运行环境等信息。并在其中加载mapper.xml（sql映射文件，配置了操作数据库的SQL语句）文件。

通过mybatis环境等配置信息构造SQLSessionFactory会话工厂。
由会话工厂创建SQLSession来操作数据库。

mybatis底层自定义了Executor执行器接口操作数据库，Executor接口有基本执行器和缓存执行器两个实现。

Mapped Statement也是mybatis一个底层封装对象，包装了mybatis配置信息及SQL映射信息等。mapper.xml文件中一个SQL对应一个Mapped Statement 对象，SQL的id即是Mapped Statement的id。

Mapped Statement对SQL执行输入参数进行定义，包括HashMap、基本类型、pojo，Executor通过Mapped Statement在执行SQL前将输入的Java对象映射至SQL中，输入参数映射就是jdbc编程中对preparedStatement设置参数。

Mapped Statement对SQL执行输出结果进行定义，包括HashMap、基本类型、pojo,Executor通过Mapped Statement在执行SQL后将输出结果映射至Java对象中，输出结果映射过程相当于jdbc编程中结果的解析处理过程。

## Hibernate

ORM，即Object-Relational Mapping，它的作用就是在关系型数据库和对象之间做了一个映射。从对象（Object）映射到关系（Relation），再从关系映射到对象。这样，我们在操作数据库的时候，不需要再去和复杂SQL打交道，只要像操作对象一样操作它就可以了（把关系数据库的字段在内存中映射成对象的属性）。


## Mybatis与Hibernate的简短比较

Mybatis和hibernate的不同之处在于它不是一个完全的ORM框架，它需要开发者自己编写SQL语句，不过mybatis可以通过xml或注解方式灵活配置要运行的SQL语句，并将Java对象和SQL语句映射生成最终执行的SQL，最后将SQL执行的结果再映射生成Java对象。

MyBatis相对于hibernate来说，学习门槛低，易于学习，灵活度也高，适合对关系数据模型要求不高的软件开发，以为其需求变化频繁。正是因为mybatis的高度灵活，所以它无法做到数据库无关性，需要实现支持多种数据库的软件则需要自定义多套SQL映射文件。

Hibernate对象/关系映射能力强，数据库无关性好，对于关系模型要求高的软件（例如需求固定的定制化软件）如果用hibernate开发可以节省很多代码，提高效率。但是Hibernate的学习门槛高，要精通门槛更高，而且怎么设计O/R映射，在性能和对象模型之间如何权衡，以及怎样用好Hibernate需要具有很强的经验和能力才行。

所以如是说：没有最好的框架，只有最适合的框架。

---

参考内容

[SSM框架系列-从JDBC到Mybatis（一）](https://www.jianshu.com/p/31116f11fa66)

[浅谈JDBC与MyBatis](https://www.jianshu.com/p/23f5220fbf72)
