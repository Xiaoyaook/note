# SpringBoot整合elasticsearch实现站内搜索

1. 同步mysql数据到elasticsearch
2. 通过关键字到elasticsearch匹配，获取到数据(有mysql索引支持)
3. 在mysql检索，业务处理，返回客户端
注意: mysql与elasticsearch数据同步(全量更新+触发更新)

[全文搜索引擎 Elasticsearch 入门教程](http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html)
阮一峰的基础教程

[Spring Data Elasticsearch](https://github.com/spring-projects/spring-data-elasticsearch)
Spring Data 对 Elasticsearch 的整合

[Spring Data Elasticsearch](https://docs.spring.io/spring-data/elasticsearch/docs/current/reference/html/)
Spring Data Elasticsearch 3.0.9文档

