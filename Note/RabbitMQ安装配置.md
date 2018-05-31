#RabbitMQ安装配置
Ubuntu16.04

## 下载,更新Erlang依赖
两种方法
### Installation using repository

#### 1. Adding repository entry
```
wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb
sudo dpkg -i erlang-solutions_1.0_all.deb
```
#### 2.  Installing Erlang
```
sudo apt update
sudo apt install erlang
```

运行以下命令查看erlang版本：
`erl --version`

### 编译安装

#### 安装依赖项

##### 1. gcc/g++、make等开发工具
 `sudo apt-get install build-essential`

##### 2. 其它Erlang用到的关键库
`sudo apt-get install libncurses5-dev m4 libssl-dev`
##### 3. 其他库
`sudo apt-get install unixodbc unixodbc-dev libc6`
##### 4. 获取源码，编译安装
```
wget http://erlang.org/download/otp_src_20.3.tar.gz
tar -zxvf otp_src_20.3.tar.gz
cd otp_src_20.3
./configure --without-javac
make
sudo make install
```
之后运行erl
```
cd bin
./erl
```

### 安装rabbitmq-server
```
echo 'deb http://www.rabbitmq.com/debian/ testing main' | sudo tee /etc/apt/sources.list.d/rabbitmq.list

wget -O- https://www.rabbitmq.com/rabbitmq-release-signing-key.asc | sudo apt-key add -

sudo apt-get update

sudo apt-get install rabbitmq-server
```

查看运行状态
`invoke-rc.d rabbitmq-server status`

看一下进程
`ps -ef | grep rabbit`

看一下端口的占用情况
`netstat -nap | grep 5672`

#### 安装RabbitMQ监控管理插件进行RabbitMQ的管理 
`sudo rabbitmq-plugins enable rabbitmq_management`

#### 安装完成后在rabbitMQ中添加用户
`rabbitmqctl add_user username password`

#### 将用户设置为管理员（只有管理员才能远程登录）
`rabbitmqctl set_user_tags username administrator`
#### 为用户设置读写等权限
`rabbitmqctl set_permissions -p / username ".*" ".*" ".*"`
#### 查看用户 
`rabbitmqctl list_users`
#### 使用firefox浏览器登录
`http://localhost:15672`
在登录页面使用 guest/guest用户名和密码登录RabbitMQ管理系统，在系统中可以对RabbitMQ服务进行channel,queue,用户等的管理
