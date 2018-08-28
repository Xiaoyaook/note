# Java 客户端操作 Elasticsearch

目前 Elasticsearch 有很多第三方 Java 客户端
如 TransportClient，Jest, [Spring Data Elasticsearch](https://github.com/spring-projects/spring-data-elasticsearch)（Spring Data 对 Elasticsearch 的整合）
还有官方的[Java REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.3/java-rest-overview.html)

## SpringBoot 对 Elasticsearch 的支持

SpringBoot 为 Elasticsearch 提供了基本的自动配置，并可以通过添加`spring-boot-starter-data-elasticsearch`依赖，来方便的收集依赖项。

目前最新的SpringBoot版本为 2.0.4.RELEASE，
2.0.4.RELEASE 版本的`spring-boot-starter-data-elasticsearch`中包含了 3.0.9.RELEASE 版本的 `spring-data-elasticsearch`

而3.0.9版本的`spring-data-elasticsearch`仅支持 5.5.0 版本的 Elasticsearch

SpringBoot 还支持使用 [Jest](https://github.com/searchbox-io/Jest) (第三方 Java Elastic Search 客户端)连接 Elasticsearch, 并可以在 application.properties 中方便地对客户端进行配置。
[Connecting to Elasticsearch by Using Jest](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-connecting-to-elasticsearch-jest)

## Java REST Client

Java REST Client 有两种版本: 
1. Java Low Level REST Client
2. Java High Level REST Client

Java高级别REST客户端（The Java High Level REST Client）以后简称高级客户端，内部仍然是基于低级客户端。它提供了更多的API，接受请求对象作为参数并返回响应对象，由客户端自己处理编码和解码。

每个API都可以同步或异步调用。 同步方法返回一个响应对象，而异步方法的名称以async后缀结尾，需要一个监听器参数，一旦收到响应或错误，就会被通知（由低级客户端管理的线程池）。 

## Java high-level REST client 

Java high-level REST client 是目前官方推荐使用的客户端，
**High Level REST Client 与 Elasticsearch 具有相同的发布周期**。
故我们能够使用最新版的 Elasticsearch。

### 兼容性

使用 High Level REST Client 与 Elasticsearch 进行通信，主版本号需要一致，次版本号不必相同，因为它是向前兼容的。次版本号小于等于Elasticsearch的都可以。这意味着它支持**与更高版本的Elasticsearch进行通信**。

### Maven 配置

高级客户端依赖于以下部件及其传递依赖关系：
* org.elasticsearch.client:elasticsearch-rest-client
* org.elasticsearch:elasticsearch

目前的最新版本
```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>6.3.2</version>
</dependency>
```
此依赖中已经包含同版本的`elasticsearch`和`elasticsearch-rest-client`

### 用一个简单的例子来演示高级客户端对 Elasticsearch 的操作

#### 创建一个实体类

```Java
public class Person {
    private String personId;
    private String name;

    // 省略setter,getter

    @Override
    public String toString() {
        return "Person{" +
                "personId='" + personId + '\'' +
                ", name='" + name + '\'' +
                '}';
    }
}
```

#### 定义连接参数

```Java
    private static final String HOST = "localhost";
    // Elasticsearch查询服务器使用9200端口，我们可以通过RESTful API直接查询数据库。
    private static final int PORT_ONE = 9200;
    // REST服务器使用9201端口，外部客户端可以使用它来连接和执行操作。
    private static final int PORT_TWO = 9201;
    // 通信方式
    private static final String SCHEME = "http";
    // 高级客户端实例
    private static RestHighLevelClient restHighLevelClient;
    // Jackson 的转换类
    private static ObjectMapper objectMapper = new ObjectMapper();
    // 索引名称
    private static final String INDEX = "persondata";
    // 索引类型
    private static final String TYPE = "person";
```

#### 建立一个连接

```Java
    /**
     * 实现了单例模式
     * 不会为ES创建多个连接，从而节省大量内存，并保证线程安全
     * @return RestHighLevelClient
     */
    private static synchronized RestHighLevelClient makeConnection() {

        if(restHighLevelClient == null) {
            restHighLevelClient = new RestHighLevelClient(
                    RestClient.builder(
                            new HttpHost(HOST, PORT_ONE, SCHEME),
                            new HttpHost(HOST, PORT_TWO, SCHEME)));
        }

        return restHighLevelClient;
    }
```

实现 RestHighLevelClient 为单例，因此与 Elasticsearch 的连接为线程安全。
初始化此连接客户端后，可以使用它来执行任何支持的API。

#### 关闭一个连接

```Java
    private static synchronized void closeConnection() throws IOException {
        restHighLevelClient.close();
        restHighLevelClient = null;
    }
```

为RestHighLevelClient对象分配了null，以便Singleton模式可以保持一致。

#### 插入数据

注意 Index Request 所需参数,以及文档源的提供方式。

有多种方式提供 document source（文档源）,下面这种就是以 Map 形式提供，可自动转换为JSON格式。

同时我们使用Java的UUID类来创建对象的唯一标识符。

```Java
    private static Person insertPerson(Person person){
        person.setPersonId(UUID.randomUUID().toString());
        Map<String, Object> dataMap = new HashMap<String, Object>();
        dataMap.put("personId", person.getPersonId());
        dataMap.put("name", person.getName());
        IndexRequest indexRequest = new IndexRequest(INDEX, TYPE, person.getPersonId())
                .source(dataMap);
        try {
            IndexResponse response = restHighLevelClient.index(indexRequest);
        } catch(ElasticsearchException e) {
            e.getDetailedMessage();
        } catch (java.io.IOException ex){
            ex.getLocalizedMessage();
        }
        return person;
    }
```

返回的 IndexResponse 包含已执行操作的信息，其中包含很多有用的信息，如文档是否为初次创建，或者是否重写了已存在的文档。

#### 获取数据

注意 GetRequest 所需的参数,其还有很多可选参数。

```Java
    private static Person getPersonById(String id){
        GetRequest getPersonRequest = new GetRequest(INDEX, TYPE, id);
        GetResponse getResponse = null;
        try {
            getResponse = restHighLevelClient.get(getPersonRequest);
        } catch (java.io.IOException e){
            e.getLocalizedMessage();
        }
        return getResponse != null ?
                objectMapper.convertValue(getResponse.getSourceAsMap(), Person.class) : null;
    }
```
Fetch Object after its update
通过`getResponse.getSourceAsMap()`我们得到的实际上是一个`Map<String, Object>`

最后 我们使用 Jackson 的 objectMapper 将此 Map 转换成一个 POJO 对象。

#### 更新数据

Update API允许使用脚本或传递部分文档来更新现有文档。

```Java
    private static Person updatePersonById(String id, Person person){
        UpdateRequest updateRequest = new UpdateRequest(INDEX, TYPE, id)
                .fetchSource(true);    // 更新后获取对象
        try {
            String personJson = objectMapper.writeValueAsString(person);
            updateRequest.doc(personJson, XContentType.JSON);
            UpdateResponse updateResponse = restHighLevelClient.update(updateRequest);
            return objectMapper.convertValue(updateResponse.getGetResult().sourceAsMap(), Person.class);
        }catch (JsonProcessingException e){
            e.getMessage();
        } catch (java.io.IOException e){
            e.getLocalizedMessage();
        }
        System.out.println("Unable to update person");
        return null;
    }
```

上面的例子中，我们没有传递需要更新的对象的任何特定属性，而是通过
`updateRequest.doc(personJson, XContentType.JSON);`
传递完整的 JSON，它将替换该对象的每个键。

#### 删除数据

删除数据时，我们也是通过Index，Type和id来定位我们的资源的。

```Java
    private static void deletePersonById(String id) {
        DeleteRequest deleteRequest = new DeleteRequest(INDEX, TYPE, id);
        try {
            DeleteResponse deleteResponse = restHighLevelClient.delete(deleteRequest);
        } catch (java.io.IOException e){
            e.getLocalizedMessage();
        }
    }
```

#### 测试

下面来实际测试一下增删改查四个操作。

```Java
    public static void main(String[] args) throws IOException {

        makeConnection();

        System.out.println("Inserting a new Person with name xiaoyaook...");
        Person person = new Person();
        person.setName("xiaoyaook");
        person = insertPerson(person);
        System.out.println("Person inserted --> " + person);

        System.out.println("Changing name to `XY`...");
        person.setName("XY");
        updatePersonById(person.getPersonId(), person);
        System.out.println("Person updated  --> " + person);

        System.out.println("Getting XY...");
        Person personFromDB = getPersonById(person.getPersonId());
        System.out.println("Person from DB  --> " + personFromDB);

        System.out.println("Deleting XY...");
        deletePersonById(personFromDB.getPersonId());
        System.out.println("Person Deleted");

        closeConnection();
    }
```

#### 总结

上面的例子，只是一部分 Document API

[Java High Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.3/java-rest-high.html) 文档中有更为详细的可选参数介绍。

以及 Search API, Indices API, Cluster API 等真正发挥 Elasticsearch 储存、搜索和分析海量数据能力的API。

## 推荐阅读

[全文搜索引擎 Elasticsearch 入门教程](http://www.ruanyifeng.com/blog/2017/08/elasticsearch.html) 阮一峰的基础教程
[ElasticSearch Tutorial for Beginners](https://www.javacodegeeks.com/2018/03/elasticsearch-tutorial-beginners.html) javacodegeeks的教程，本文的例子来自于此。
[使用Java High Level REST Client操作elasticsearch](https://www.cnblogs.com/ginb/p/8716485.html) 对一部分官方文档的翻译
[Java High Level REST Client](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/6.3/java-rest-high.html) 官方文档