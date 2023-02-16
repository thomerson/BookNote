## 分布式session

1. 使用```JWT```存储用户信息，不适用session ⭐

2. 客户端存储

    cookie中存放session信息

    缺点：

    * 数据存储在客户端，存在安全隐患
    * cookie存储大小、类型存在限制
    * 数据存储在cookie中，如果一次请求cookie过大，会给网络增加更大的开销

3. session复制

    小型企业应用使用较多的一种服务器集群session管理机制

    在同一个局域网里面通过发送广播来异步同步session，```Tomcat```可以配置

4. session黏性

    使用```nginx```进行session绑定

    基于nginx的ip-hash策略，可以对客户端和服务器进行绑定，同一个客户端就只能访问该服务器，无论客户端发送多少次请求都被同一个服务器处理

    缺点：

    * 容易造成单点故障，如果有一台服务器宕机，那么该台服务器上的session信息将会丢失
    * 前端不能有负载均衡，如果有，session绑定将会出问题

    优点：

    * 配置简单

5. 基于redis存储session方案⭐

    ![session-redis](https://img-blog.csdnimg.cn/2befba0a53ad413c9c669ddc4b3d7a17.png)

    java使用```Spring Session```

    [.net core基于redis存储session方案]()


