# Ubuntu16.04部署Nginx+gunicorn

按照这篇博文进行服务器部署[使用 Nginx 和 Gunicorn 部署 Django 博客](https://www.zmrenwu.com/post/20/)

按照这篇博文设置系统自启动gunicorn[ubuntu16.04自动系统启动gunicorn](http://www.yangfan.cc/zhanzhang/1425.html)

## 使用putty连接远程服务器
安装putty

`sudo apt-get install putty`
需要的依赖直接下载即可

## 设置新用户
第一次登录服务器,我们以root用户登录,在 root 下部署代码不安全，最好是建一个新用户
下列命令将创建一个超级用户
> `useradd -m -s /bin/bash xiaoyaook`
这条命令创建一个新用户,xiaoyaook是用户名,可任意取
`usermod -a -G sudo xiaoyaook`
这条命令把新创建的用户加入超级权限组
`passwd xiaoyaook`
这条命令给新用户设置密码
`su - xiaoyaook`
这条命令切换用户

## 安装软件
新用户创建并切换成功了。如果是新服务器的话，最好先更新一下系统，避免因为版本太旧而给后面安装软件带来麻烦。运行下面的两条命令：
```
sudo apt-get update
sudo apt-get upgrade
```

接下来就可以安装必要的软件了，这里我们需要用到的软件有 Nginx、Python3、Git、pip 和 virtualenv.

```
sudo apt-get install nginx
sudo apt-get install git python3 python3-pip
sudo pip3 install virtualenv
```

## 解析域名到服务器的 IP 地址
将域名和服务器的 IP 地址绑定后，用户就可以通过在浏览器输入域名来访问服务器.

我这里使用阿里云,按照阿里云的指引走即可.

## 启动 Nginx 服务
运行下面的命令启动 Nginx 服务：
`sudo service nginx start`
在浏览器输入域名，看到 Welcome to nginx! 说明 Nginx 启动成功了。
