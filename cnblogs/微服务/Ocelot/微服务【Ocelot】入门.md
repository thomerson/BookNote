## Ocelot

.NET Core实现并且开源的API网关技术

一系列按特定顺序排列的中间件

### 功能

* 路由
* [请求聚合]()
* [服务发现]()
* [认证鉴权]()
* [限流熔断]()
* 缓存
    * [微服务【Ocelot】集成Cache]()
    * [微服务【Ocelot】集成Cache使用CacheManager]()
    * [微服务【Ocelot】集成Cache使用Redis]()
    * [微服务【Ocelot】集成Cache自定义]()
* [服务治理]()
* 内置了负载均衡器、Service Fabric、Skywalking等的集成


### 集成

* [IdentityServer]()

    OcelotIndentityServer

* 网关集群配置

    OcelotMultipleInstances

* [结合Consul服务发现]()

    OcelotMultipleInstancesConsul

* 结合Service Fabric

    OcelotServiceFabric

* 集成CacheManager做缓存

    集成CacheManager配合Redis做分布式缓存

    自定义缓存

* [集成Polly做服务治理]()

    接口超时、访问异常、并发量大

