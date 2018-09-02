# MySQL学习

## MySQL基本命令

### 导入*.sql脚本
在MySQL Qurey Brower中直接导入*.sql脚本，是不能一次执行多条sql命令的，在mysql中执行sql文件的命令:
`source ~/balabala/*.sql`

## 库操作

### 创建数据库
`create database <数据库名>`

### 列出所有数据库
`show databases;`

### 连接数据库
`use '数据库名';`

### 删除数据库
`drop database 数据库名;`

### 查看当前使用的数据库
`select database();`

## 表操作，操作之前应连接某个数据库

### 建表
`create table <表名> ( <字段名1> <类型1> [,..<字段名n> <类型n>]);`

### 重命名表
`ALTER  TABLE table_name RENAME TO new_table_name`

### 列出当前数据库包含的表信息
`show tables;`

### 删除数据表
`drop table 数据表名;`

### 列出表的所有字段
`show full columns from <表名>`

### 显示数据表结构
`describe 表名;`

## 增删查改

### 插入数据
`INSERT INTO table_name ( field1, field2,...fieldN ) VALUES ( value1, value2,...valueN );`

### 查询数据
`SELECT column_name,column_name
FROM table_name[WHERE Clause][OFFSET M ][LIMIT N]`

查询语句中你可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件。

SELECT 命令可以读取一条或者多条记录。

你可以使用星号（*）来代替其他字段，SELECT语句会返回表的所有字段数据

你可以使用 WHERE 语句来包含任何条件。

你可以通过OFFSET指定SELECT语句开始查询的数据偏移量。默认情况下偏移量为0。

你可以使用 LIMIT 属性来设定返回的记录数。

### WHERE 子句
`SELECT field1, field2,...fieldN FROM table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....`

查询语句中你可以使用一个或者多个表，表之间使用逗号, 分割，并使用WHERE语句来设定查询条件。

你可以在 WHERE 子句中指定任何条件。

你可以使用 AND 或者 OR 指定一个或多个条件。

WHERE 子句也可以运用于 SQL 的 DELETE 或者 UPDATE 命令。

### UPDATE 查询
`UPDATE table_name SET field1=new-value1, field2=new-value2[WHERE Clause]`

你可以同时更新一个或多个字段。

你可以在 WHERE 子句中指定任何条件。

你可以在一个单独表中同时更新数据。

### DELETE 语句
`DELETE FROM table_name [WHERE Clause]`
如果没有指定 WHERE 子句，MySQL 表中的所有记录将被删除。

你可以在 WHERE 子句中指定任何条件

您可以在单个表中一次性删除记录。

### LIKE 子句
`SELECT field1, field2,...fieldN 
table_name1, table_name2...WHERE field1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'`

你可以在 WHERE 子句中指定任何条件。

你可以在 WHERE 子句中使用LIKE子句。

你可以使用LIKE子句代替等号 =。

LIKE 通常与 % 一同使用，类似于一个元字符的搜索。

你可以使用 AND 或者 OR 指定一个或多个条件。

你可以在 DELETE 或 UPDATE 命令中使用 WHERE...LIKE 子句来指定条件

### UNION 操作符
MySQL UNION 操作符用于连接两个以上的 SELECT 语句的结果组合到一个结果集合中。多个 SELECT 语句会删除重复的数据。

```
SELECT expression1, expression2, ... expression_n
FROM tables[WHERE conditions]UNION 
[ALL | DISTINCT]SELECT expression1, expression2, ... expression_n FROM tables[WHERE conditions];
```
参数：
* expression1, expression2, ... expression_n: 要检索的列。
* tables: 要检索的数据表。
* WHERE conditions: 可选， 检索条件。
* DISTINCT: 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响。
* ALL: 可选，返回所有结果集，包含重复数据。

### 排序
`SELECT field1, field2,...fieldN table_name1, table_name2...ORDER BY field1,
 [field2...] [ASC [DESC]]`

 你可以使用任何字段来作为排序的条件，从而返回排序后的查询结果。

你可以设定多个字段来排序。

你可以使用 ASC 或 DESC 关键字来设置查询结果是按升序或降序排列。 默认情况下，它是按升序排列。

你可以添加 WHERE...LIKE 子句来设置条件。

### GROUP BY 语法
GROUP BY 语句根据一个或多个列对结果集进行分组。

在分组的列上我们可以使用 COUNT, SUM, AVG,等函数。

```
SELECT column_name, function(column_name)FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

### 在 SELECT, UPDATE 和 DELETE 语句中使用 Mysql 的 JOIN 来联合多表查询
JOIN 按照功能大致分为如下三类：
* **INNER JOIN（内连接,或等值连接）**：获取两个表中字段匹配关系的记录。
* **LEFT JOIN（左连接）**：获取左表所有记录，即使右表没有对应匹配的记录。
* **RIGHT JOIN（右连接）**： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。

### ALTER命令
作用：
* 删除，添加或修改表字段
* 修改字段类型及名称
* 修改字段默认值
* Null 值处理
* 修改表名

### 索引

索引分单列索引和组合索引。单列索引，即一个索引只包含单个列，一个表可以有多个单列索引，但这不是组合索引。组合索引，即一个索包含多个列。

创建索引时，你需要确保该索引是应用在 SQL 查询语句的条件(一般作为 WHERE 子句的条件)

#### 普通索引

创建方式：
* 创建索引 : `CREATE INDEX indexName ON mytable(username(length));`
    * 如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length。
* 修改表结构(添加索引) : `ALTER mytable ADD INDEX [indexName] ON (username(length))`
* 创建表的时候直接指定 : `CREATE TABLE mytable
(ID INT NOT NULL,username VARCHAR(16) NOT NULL, INDEX [indexName] (username(length)));`   
* 删除索引的语法 : `DROP INDEX [indexName] ON mytable;`

#### 唯一索引

索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。

创建方式：

* 创建索引 : `CREATE UNIQUE INDEX indexName ON mytable(username(length))`
* 修改表结构 : `ALTER table mytable ADD UNIQUE [indexName] (username(length))`
* `创建表的时候直接指定` : `CREATE TABLE mytable
(ID INT NOT NULL,username VARCHAR(16) NOT NULL, UNIQUE [indexName] (username(length)));`

#### 使用ALTER 命令添加和删除索引

有四种方式来添加数据表的索引：

* `ALTER TABLE tbl_name ADD PRIMARY KEY (column_list)`: 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL。
* `ALTER TABLE tbl_name ADD UNIQUE index_name (column_list)`: 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）。
* `ALTER TABLE tbl_name ADD INDEX index_name (column_list)`: 添加普通索引，索引值可出现多次。
* `ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list)`:该语句指定了索引为 FULLTEXT ，用于全文索引。

#### 使用 ALTER 命令添加和删除主键

主键只能作用于一个列上，添加主键索引时，你需要确保该主键默认不为空（NOT NULL）.

删除主键时只需指定PRIMARY KEY，但在删除索引时，你必须知道索引名。

#### 显示索引信息
可以使用 SHOW INDEX 命令来列出表中的相关的索引信息。可以通过添加 \G 来格式化输出信息。

`mysql> SHOW INDEX FROM table_name; \G........`

## mysql 执行计划explain详解

explain主要是用来获取一个query的执行计划，描述mysql如何执行查询操作、执行顺序，使用到的索引，以及mysql成功返回结果集需要执行的行数。可以帮助我们分析 select 语句,让我们知道查询效率低下的原因,从而改进我们的查询，让查询优化器能够更好的工作。

## 子查询与关联查询的区别

### 关联查询
在实际应用中，经常需要在一个查询语句中显示多张表的数据，这种多表数据记录关联查询，简称关联查询。

### 子查询
在一个表表达中可以调用另一个表表达式，这个被调用的表表达式叫做子查询（subquery），我么也称作子选择（subselect）或内嵌选择（inner select）。子查询的结果传递给调用它的表表达式继续处理。