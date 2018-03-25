# MySQL添加用户、删除用户与授权

## 登录MySQL
`mysql -u root -p`

`password`

## 新建用户
`mysql> insert into mysql.user(Host,User,Password) values("localhost","test",password("1234"));`

这样就创建了一个名为：test 密码为：1234 的用户。

注意 ：**此处的"localhost"，是指该用户只能在本地登录，不能在另外一台机器上远程登录。如果想远程登录的话，将"localhost"改为"%"，表示在任何一台电脑上都可以登录。也可以指定某台机器可以远程登录。**

## 为用户授权
授权格式：grant 权限 on 数据库.* to 用户名@登录主机 identified by "密码";

首先登录MySQL（有ROOT权限），或以ROOT身份登录.

### 授权test用户拥有testDB数据库的所有权限（某个数据库的所有权限）：
`mysql>grant all privileges on testDB.* to test@localhost identified by '1234';`

`mysql>flush privileges;//刷新系统权限表`

### 如果想指定部分权限给一用户，可以这样来写:
`mysql>grant select,update on testDB.* to test@localhost identified by '1234';`

`mysql>flush privileges; //刷新系统权限表`

### 授权test用户拥有所有数据库的某些权限:
`mysql>grant select,delete,update,create,drop on *.* to test@"%" identified by "1234";`

@"%" 表示对所有非本地主机授权，不包括localhost.

对localhost授权：加上一句
`grant all privileges on *.* to test@localhost identified by '1234';`即可

## 删除用户
首先登录MySQL（有ROOT权限），或以ROOT身份登录.

`mysql>Delete FROM user Where User='test' and Host='localhost';`

`mysql>flush privileges;`

删除账户及权限：
`>drop user 用户名@'%';`

`>drop user 用户名@localhost;`

## 修改指定用户密码
首先登录MySQL（有ROOT权限），或以ROOT身份登录.

`mysql>update mysql.user set password=password('新密码') where User="test" and Host="localhost";`

`mysql>flush privileges;`

