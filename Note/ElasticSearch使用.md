# ElasticSearch 使用

## 下载

[Download Elasticsearch](https://www.elastic.co/downloads/elasticsearch) 按照安装步骤进行操作：

1. 下载并解压
2. `source bin/elasticsearch`
3. 访问 `http://localhost:9200/`

此时我们可以看到返回了一个JSON形式的数据，证明已经启动成功。

## 插件配置

[elasticsearch-head](https://github.com/mobz/elasticsearch-head) 一个图形化 elasticsearch 插件

1. 下载并解压
2. 进入解压后的文件夹，`npm install`
3. `npm run start` 运行服务
4. 访问 `http://localhost:9100/`

由于浏览器访问会出现跨域问题，所以我们要给 elasticsearch 进行跨域配置

1. `vim config/elasticsearch.yml`
2. 在文件末尾添加`http.cors.enabled: true`和`http.cors.allow-origin: "*"`

## 分布式安装

首先还是添加一些 elasticsearch 配置

主
```yml
cluster.name: wali
node.name: master
node.master: true

network.host: 127.0.0.1
```
从
```yml
cluster.name: wali
node.name: slave1

network.host: 127.0.0.1
http.port: 8200

discovery.zen.ping.unicast.hosts: ["127.0.0.1"]
```

同时启动主从程序，在可视化页面可以看到我们的集群已经运行。

## 索引
创建
增删改查
