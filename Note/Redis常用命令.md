# Redis常用命令

## key/value
* SET key value : 持久化储存键值对
* GET key : 由key取值
* DEL key : 删除所给的key和其相对应的value
* SETNX key value : 仅当所给的key不存在时,储存键值对
* INCR key : key所对应的值加1(原子化操作)
* EXPIRE key : 设置key的存活时间
* TTL key : 查询key还能存活多长时间(-1意味着持久存在,-2意味着该key已经不存在)

## list
* RPUSH listname value : 把value加到列表尾部
* LPUSH listname value : 把value加到列表头部
* LRANGE listname index index : 从两个index间取元素
* LLEN listname : 返回列表长度
* LPOP listname : 移除列表的第一个元素并将其返回
* RPOP listname : 移除列表的最后一个元素并将其返回

## set
* SADD setname value : 将给定值加入到集合中
* SREM setname value : 将给定值从集合中移除
* SISMEMBER setname value : 验证给定值是否存在于集合中,若存在返回1,否则返回0
* SMEMBERS setname : 返回由所有集合元素组成的列表
* SUNION setname setname... :  组合两个及以上集合,并返回由所有元素组成的列表

## sorted set
类似于常规的set, 但是其每个value都有个与之对应的score,score用来对集合中的元素排序

## Hashes
hashs可以映射多对键值,是一种很适合表示对象的数据类型

* HSET hashsname key value : 保存键值对
* HGETALL hashsname : 获取所有储存数据
* HMSET hashsname key value key value... : 同时保存多对键值对
* HGET hashsname key : 只取出单一域的值
* Hashs也可以进行原子化的数值操作,详细命令见文档

[Redis官方文档](https://redis.io/documentation)