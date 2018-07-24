# HTTP缓存

Web 缓存大致可以分为：数据库缓存、服务器端缓存（代理服务器缓存、CDN 缓存）、浏览器缓存。

浏览器缓存也包含很多内容： HTTP 缓存、indexDB、cookie、localstorage 等等。

与 HTTP 缓存相关的几个术语：
* 缓存命中率：从缓存中得到数据的请求数与所有请求数的比率。理想状态是越高越好。
* 过期内容：超过设置的有效时间，被标记为“陈旧”的内容。通常过期内容不能用于回复客户端的请求，必须重新向源服务器请求新的内容或者验证缓存的内容是否仍然准备。
* 验证：验证缓存中的过期内容是否仍然有效，验证通过的话刷新过期时间。
* 失效：失效就是把内容从缓存中移除。当内容发生改变时就必须移除失效的内容。

浏览器对静态资源的HTTP缓存有两种情况，一种是强缓存(本地缓存)，另一种是弱缓存(协商缓存)。

浏览器请求一个静态资源时的HTTP流程：
* 强缓存阶段：先在本地查找该资源，如果发现该资源，并且其他限制也没有问题(比如:缓存有效时间)，就命中强缓存，返回200，直接使用强缓存，并且不会发送请求到服务器
* 弱缓存阶段：在本地缓存中找到该资源，发送一个http请求到服务器，服务器判断这个资源没有被改动过，则返回304，让浏览器使用该资源。
* 缓存失败阶段(重新请求)：当服务器发现该资源被修改过，或者在本地没有找到该缓存资源，服务器则返回该资源的数据。

缓存流程图可以在最下端的**引用文章**中找到。

强缓存与弱缓存的区别：
* **获取资源形式**： 都是从缓存中获取资源的。
* **状态码**： 强缓存返回200(from cache),弱缓存返回304状态码
* **请求(最大区别)**：
    * 强缓存不发送请求，直接从缓存中取。
    * 弱缓存需要发送一个请求，验证这个文件是否可以使用（有没有被改动过）。

## 强缓存

强缓存是利用Expires或者Cache-Control，让原始服务器为文件设置一个过期时间，在多长时间内可以将这些内容视为最新的。

若时间未过期，则命中强缓存，使用缓存文件不发送请求。

### Pragma

在 http 1.0 时代，给客户端设定缓存方式可通过两个字段 Pragma 和 Expires，虽然这两个字段早可抛弃，但为了做 http 协议的向下兼容，你还是可以看到很多网站依然带上这两个字段。

在响应报文中当该字段值为 "no-cache" 的时候，会通知客户端不要对该资源进行缓存，每次都得向服务器发一次请求才行。

### Expires

有了 Pragma 来禁用缓存，那如果需要设置缓存的话就得有个东西来设置缓存的时间，对于 http 1.0 来说，Expires 就是做这件事的。

Expires 的值对应一个 GMT (格林尼治时间)，比如 “Mon, 22 Mar 2017 11:12:01 GMT” 来告诉浏览器资源缓存过期时间，如果还没有超过该时间点则不发请求，直接返回 200 OK。(from cache)

所以如果面试官问你 http 响应 200 OK (from cache) 是否有发送 http 请求，那么就请回答没有，它们都是直接读取缓存资源的。

当两者一起使用时候，Pragma 优先级更高，即当 Pragma 设置禁用缓存时，又给 Expires 定义一个还未到期的时间，会发现仍然会发送新的请求，所以 Pragma 优先级比 Expires 高。

当然现在不提倡这样的方式，因为它有两个致命的**缺点**：

1. Expires 定义的缓存时间是相对于服务器上的时间而言的，而浏览器在判断的时候是基于客户端的系统时间的，如果用户修改了自己电脑的系统时间，那么这个缓存时间将没有任何意义。
2. 假如客户端上某个资源缓存时间过期了，但此时其实服务器并没有更新过该资源，那么这时候客户端要求服务器重新把东西再发送过来一遍，会浪费带宽和时间，这显然是不合理的，我们需要有一种正确的机制用来判断东西到底可以直接使用缓存。

### Cache-Control

Cache-Control 是http1.1中为了弥补Expires的缺陷而加入的，当Expires和Cache-Control同时存在时，Cache-Control优先级高于Expires。

Cache-Control 可以由多个字段组合而成，主要有以下几个取值：

* public: 表明响应可以被任何对象（包括：发送请求的客户端，代理服务器，等等）缓存。
* private: 只有用户自己的浏览器能够进行缓存，公共的代理服务器不允许缓存。
* no-cache: 强制浏览器在使用cache拷贝之前先提交一个http请求到源服务器进行确认。http请求没有减少，会减少一个响应体(文件内容),这种个选项类似弱缓存。
* only-if-cached: 表明客户端只接受已缓存的响应，并且不要向原始服务器检查是否有更新的拷贝。
* max-age=60：设置缓存存储的最大周期，超过这个时间缓存被认为过期(单位秒)。 这里是60秒
* no-store: 告诉浏览器在任何情况下都不要进行cache，不在本地保留拷贝。
* must-revalidate: 缓存必须在使用之前验证旧资源的状态，并且不可使用过期资源。

## 协商缓存

缓存的资源到期了，并不意味着资源内容发生了改变，如果和服务器上的资源没有差异，实际上没有必要再次请求。客户端和服务器端通过某种验证机制验证当前请求资源是否可以使用缓存。

浏览器第一次请求数据之后会将数据和响应头部的缓存标识存储起来。再次请求时会带上存储的头部字段，服务器端验证是否可用。如果返回 304 Not Modified，代表资源没有发生改变可以使用缓存的数据，获取新的过期时间。反之返回 200 就相当于重新请求了一遍资源并替换旧资源。

### Last-Modified & if-modified-since

Last-Modified与If-Modified-Since是一对报文头，属于http 1.0。

last-modified是web服务器认为文件的最后修改时间，last-modified是第一次请求文件的时候，服务器返回的一个属性。

Last-modified: 服务器端资源的最后修改时间，响应头部会带上这个标识。第一次请求之后，浏览器记录这个时间，再次请求时，请求头部带上 If-Modified-Since 即为之前记录下的时间。服务器端收到带 If-Modified-Since 的请求后会去和资源的最后修改时间对比。若修改过就返回最新资源，状态码 200，若没有修改过则返回 304。

**注意**: 如果响应头中有 Last-modified 而没有 Expire 或 Cache-Control 时，浏览器会有自己的算法来推算出一个时间缓存该文件多久，不同浏览器得出的时间不一样，所以 Last-modified 要记得配合 Expires/Cache-Control 使用。

### ETag & If-None-Match

ETag与If-None-Match是一对报文，属于http 1.1。

**ETag是一个文件的唯一标志符**。就像一个哈希或者指纹，每个文件都有一个单独的标志，只要这个文件发生了改变，这个标志就会发生变化。

Etag/lastModified过程如下: 

1. 客户端第一次向服务器发起请求,服务器将附加Last-Modified/ETag到所提供的资源上去
2. 当再一次请求资源,如果没有命中强缓存,在执行在验证时,将上次请求时服务器返回的Last-Modified/ETag一起传递给服务器。
3. 服务器检查该Last-Modified或ETag，并判断出该资源页面自上次客户端请求之后还未被修改，返回响应304和一个空的响应体。

**如果 Last-Modified 和 ETag 同时被使用，则要求它们的验证都必须通过才会返回304，若其中某个验证没通过，则服务器会按常规返回资源实体及200状态码**。

 last-modified 和 Etag 区别:
* 某些服务器不能精确得到资源的最后修改时间，这样就无法通过最后修改时间判断资源是否更新。
Last-modified 只能精确到秒。
* 一些资源的最后修改时间改变了，但是内容没改变，使用 Last-modified 看不出内容没有改变。
* Etag 的精度比 Last-modified 高，属于强验证，要求资源字节级别的一致，优先级高。如果服务器端有提供 ETag 的话，必须先对 ETag 进行 Conditional Request。

## 实际应用

考虑缓存的内容：
* css样式文件
* js文件
* logo、图标
* html文件
* 可以下载的内容

不应该被缓存的内容: 
* 业务敏感的 GET 请求

### SpringBoot中的应用

通过Spring的缓存机制来缓存静态文件，在Spring Boot中配置静态文件缓存只需要在配置文件中添加一些配置。

下面给出一个示例配置：
```
spring.resources.add-mappings=true
spring.resources.cache.cachecontrol.max-age=3600
spring.resources.chain.cache=true
spring.resources.chain.enabled=true
spring.resources.chain.gzipped=true
spring.resources.chain.html-application-cache=true
spring.resources.static-locations=classpath:/static/
```

详细配置可参考官方文档[SPRING RESOURCES HANDLING](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)

---

参考文章：
[HTTP 缓存机制一二三](https://zhuanlan.zhihu.com/p/29750583)
[谈谈 HTTP 缓存](https://zhuanlan.zhihu.com/p/25647421)
[当我们在谈论HTTP缓存时我们在谈论什么](https://zhuanlan.zhihu.com/p/37975375)
