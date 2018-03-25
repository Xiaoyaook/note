# MySQL基本命令

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

### 列出当前数据库包含的表信息
`show tables;`

### 删除数据表
`drop table 数据表名;`

### 显示数据表结构
`describe 表名;`

## 增删查改

### 插入数据
`insert into <表名> [( <字段名1>[,..<字段名n > ])] values ( 值1 )[, ( 值n )]`

