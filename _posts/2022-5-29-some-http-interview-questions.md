# HTTP 常见面试问题

## HTTP常见状态码有哪些？常见字段有哪些？

常见状态码：
1. **2XX** 表示server成功处理了来自client的请求， 最常见的是200
2. **3XX** 表示资源重定向，301临时重定向，302永久重定向，**303**资源重定向在本地，主要用于实现缓存技术
3. **4XX** client的请求错误； 常见的404是client请求的文件不存在，403位某资源client没有权限访问
4. **5XX** server端出错； 常见的501“即将开业，敬请期待”， 503表示服务器正忙，稍后重试

常见的字段：

1. host
2. content-length
3. content-type
4. （server返回的response中）用于缓存的一系列字段，比如cache-control， Last-modify 




## HTTP request和response格式？ 常见method有哪些？ GET和POST有什么区别？

HTTP request:

request line: method URL version
request header: key:value

HTTP response:

response line: version status-code reason phrase

常见method:
1. GET
2. POST
3. PATCH
4. DELETE
5. PUT
   
diff GET and POST:

GET: 请求资源
POST: 对server上的资源进行修改

所以GET为安全,幂等, 访问server不会对server上的数据进行破坏,每次GET的资源都是一样的; 但是POST方法,由于会对server上的数据进行更改,所以会造成破坏,并且每次访问完之后数据不一样一样.


## HTTP1.0 -> HTTP1.1 -> HTTPS -> HTTP/2 -> HTTP/3 有什么特点解决了什么问题？

### HTTP1.0
每次client和server之间传送一次数据,都需要进行一次TCP三次握手,传送完数据之后需要TCP四次挥手

### HTTP1.1
改进HTTP1.0,采用**长连接**的方式,只要client或者server中没有提出明确的断开连接操作,连接会一直保持,除非长时间没有信息传送; 由于采用长连接的方式,使得HTTP1.1可以在此基础上进行**管道网络传输**(解决了请求队头堵塞的问题,但是没有解决响应队头堵塞的问题); cons:不能对header进行压缩, 按照请求顺序响应,没有优先级

### HTTPS
HTTP1.0和HTTP1.1都是明文传送,一旦抓包,就可以看见HTTP的数据,非常不安全,所以在应用层HTTP和传输层TCP之间加了一个SSL/TLS,进行加密.
1. 在传输过程中采用**混合加密**,避免了窃听风险
    - 通信前,非对称加密
    - 通信过程中采用对称加密
2. 在传输开始时采用**摘要算法**,避免了篡改风险,保证传输的内容没有被修改
3. 对server进行验证,使用**数字证书**,保证传输的对象没有被冒充

另外, HTTP服务的默认端口为80, HTTPS服务的默认端口为443

### HTTP/2
可以压缩头部, 对多个请求重复的头部消除; 数据流传送,多路复用,一个连接中并发多个请求,但是不用按照顺序一一回应; 基于TCP,所以在应用层中解决了响应的对头阻塞问题,但是在传输层,TCP连接仍然是按照顺序传送的,传输层中存在响应队头阻塞问题;

### HTTP/3
基于UDP + QUIC协议

QUIC协议可以更快速的建立连接, 合并TLS的三次握手(具体我也不太清楚)

## HTTP缓存怎么实现？ 什么是强制缓存，什么是协商缓存？

将对应的请求响应数据缓存在本地

**强制缓存:**

第一次请求时,响应中返回资源的同时,返回一个header: cache-control, 设置当前资源的过期时间, 响应返回200状态码

之后的请求,只要cache-control中的时间没有过期,就可以直接使用本地资源,返回303状态码.

**协商缓存**

在强制缓存的前提下, 有两种方式, 表现在响应头部中的时间分别为Last-Modified 和 ETag(优先)

基于时间实现: 
如果本地缓存已过期, 再访问server时,request里会带上if-modified-since的header,里面备注该资源上一次返回时的修改时间,对比server中的资源修改时间;如果已经发生新的修改,在response中添加Last-Modified header, 更新client中的资源修改时间;

基于唯一标识实现:
当资源过期时,浏览器发现之前的response中带有Etag header, 则设置自己的reqeust header: if-none-match的value为Etag值,当server发现该头部时,会对比本地的资源,如果发生变化,则会生成新的Etag值返回,并返回新资源


