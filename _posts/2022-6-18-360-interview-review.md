# 360 interview review

### 介绍一下web server项目的整体框架:
A: proxy -> server
 1. proxy; (cache + load balancer)
      - 作为client和server的中间件, 可以用于缓存 Web 对象，方法是存储来自sever的对象的本地副本，当有新的连接需要获取server资源时,优先访问cache,只有cache不命中时才会访问server,减少了与server的通信次数; 这里采用LRU作为evict strategy
      -  load balancer; 实现了一个负载均衡器,可以选择两种策略,一个是round robin, 一个是least connection;
         - round robin: 将负载平均分给每一个server,采用轮询的方式,将所有server用一个list存储, 每次有新的连接到来的时候,都选择下一个alive server进行信息处理;
         - least connection:将新的连接分配到当前连接数最少的server中;
 2. server 服务器可以对url进行解析, 并提供静态和动态页面服务, 如果需要传递参数来获取动态页面, 通过url进行传参.


### Q: GET 和 POST的区别?
A:根据 RFC 规范
 1. GET 的语义是从服务器获取指定的资源，这个资源可以是静态的文本、页面、图片视频等。GET 请求的参数位置一般是写在 URL 中，URL 规定只能支持 ASCII，所以 GET 请求的参数只允许 ASCII 字符 ，而且浏览器会对 URL 的长度有限制（HTTP协议本身对 URL长度并没有做任何规定）。
 2. POST 的语义是根据请求负荷（报文body）对指定的资源做出处理，具体的处理方式视资源类型而不同。POST 请求携带数据的位置一般是写在报文 body 中， body 中的数据可以是任意格式的数据，只要客户端与服务端协商好即可，而且浏览器不会对 body 大小做限制。

### 大唐杯 竞赛
A: 
 移动通信领域的一个知识储备竞赛, 包括移动通信基础理论,大数据常用平台的基础理论部分和仿真部分(虚拟5G基站搭建)

### 大创
A:
 做了一个体感VR手套, 通过手套上传感器的数据, 利用knn算法,训练之后,能够在vr头显中显示手套的姿势;  