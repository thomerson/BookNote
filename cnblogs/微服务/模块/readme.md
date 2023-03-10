## 微服务模块

![微服务模块](https://img2020.cnblogs.com/blog/824291/202004/824291-20200406223103730-527609758.jpg)

* [服务注册与发现]()

    * Consul

* [网关]()

    * Ocelot

* [认证授权]

    * IdentityServer4

* [服务间通信]

    * gRPC

    * REST

    * graphQL

* [服务治理]

    * Polly

* [NoSQL]

    * Redis

    * MongoDB

* [消息队列]

    * RabbitMQ

    * Kafka

* [数据一致性](#https://github.com/thomerson/BookNote/blob/master/cnblogs/dotnetore/工作应用/.NetCore【工作应用】分布式事务.md)

    * CAP

    * EventBus/事件总线

        事件总线是对观察者（发布-订阅）模式的一种实现。它是一种集中式事件处理机制，允许不同的组件之间进行彼此通信而又不需要相互依赖，达到一种解耦的目的。

        Event Bus 目的是消息解耦，不要让服务之间直接的链接。不同与SOA的服务总线，事件总线相对比较轻量，经常基于消息队列引擎进行解耦，目的是为了让服务之间的关联弱化，不直接进行关联。很多时候用的是相对稳定、可靠、企业级的RabbitMQ。

* [配置中心]

    * Apollo

* [后台任务]

    * quartz

    * HangFire


* [系统日志]

    * ExceptionLess

    * ELK

* [链路追踪]

    * SkyWalking

    * Butterfly


* [监控]

    * Zabbix

    * Prometheus

    * sqldependency


* [反向代理]()

    * Nginx


* [容器]

    * docker


* [容器编排]

    * k8s

    * Service Fabric

* [高可用]()

    * keepalived
    
## 其他相关

* 代码管理

    * nuget

    * git

    * svn

    * 代码规范


* 文档管理

    * vuePress

    * vivi

    * confluence


* [CI/CD]()

    * gitlab

    * Jenkins

    * docker


* 后端服务

    * IoC

        * Ninject


    * 对象映射

        * AutoMapper