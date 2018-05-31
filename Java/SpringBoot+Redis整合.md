# SpringBoot+Redis整合

## 配置pom
```
        <!-- redis 依赖 -->
		<dependency>
			<groupId>redis.clients</groupId>
			<artifactId>jedis</artifactId>
			<version>2.9.0</version>
		</dependency>
```

## 配置application.properties
```
#redis
redis.host=127.0.0.1
redis.port=6379
redis.timeout=3
redis.password=123456
redis.poolMaxTotal=10
redis.poolMaxIdle=10
redis.poolMaxWait=3
```

## 以注解的方式加载配置
省略get,set方法
```Java
@Component
@ConfigurationProperties(prefix = "redis")
public class RedisConfig {
    private String host;
    private int port;
    private int timeout;//秒
    private String password;
    private int poolMaxTotal;
    private int poolMaxIdle;
    private int poolMaxWait;//秒
}
```

## JedisPool
为了利用JedisPool,创建一个bean,来使JedisPool可以被自动装配.
```Java
@Service
public class RedisPoolFactory {
    @Autowired
    RedisConfig redisConfig;

    @Bean
    public JedisPool JedisPoolFactory() {
        JedisPoolConfig poolConfig = new JedisPoolConfig();
        poolConfig.setMaxIdle(redisConfig.getPoolMaxIdle());
        poolConfig.setMaxTotal(redisConfig.getPoolMaxTotal());
        poolConfig.setMaxWaitMillis(redisConfig.getPoolMaxWait() * 1000);
        JedisPool jp = new JedisPool(poolConfig, redisConfig.getHost(), redisConfig.getPort(),
                redisConfig.getTimeout()*1000, redisConfig.getPassword(), 0);
        return jp;
    }
}
```

详细请查看[官方文档](https://github.com/xetorthio/jedis/wiki/Getting-started)