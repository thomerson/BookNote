![微服务](https://img2020.cnblogs.com/blog/824291/202004/824291-20200406223103730-527609758.jpg)

## 主要模块

1. 服务注册与发现
    * [Consul]()
    * ZooKeeper
    * etcd
    * Eureka

2. 网关

    * [Ocelot]()

3. RPC

    * [gRPC]()

* 服务监控
* 服务跟踪
* 服务治理
    * 缓存，限流，熔断，链路追踪 



## 数据一致性

CAP

## EventBus

事件总线

事件总线是对观察者（发布-订阅）模式的一种实现。它是一种集中式事件处理机制，允许不同的组件之间进行彼此通信而又不需要相互依赖，达到一种解耦的目的。


Event Bus 目的是消息解耦，不要让服务之间直接的链接。不同与SOA的服务总线，事件总线相对比较轻量，经常基于消息队列引擎进行解耦，目的是为了让服务之间的关联弱化，不直接进行关联。很多时候用的是相对稳定、可靠、企业级的RabbitMQ。




https://www.cnblogs.com/xhznl/p/13091750.html

https://www.cnblogs.com/xhznl/p/13259036.html
https://www.cnblogs.com/xhznl/p/13154851.html


https://blog.csdn.net/qq_45534015/article/details/115193096

https://blog.51cto.com/u_15127590/3255103

https://www.freesion.com/article/12891241191/


http://www.identityserver.com.cn/Home/Detail/openidConnect

https://github.com/IdentityServer/IdentityServer4/tree/main/samples/Quickstarts

https://identityserver4.readthedocs.io/en/latest/quickstarts/2_interactive_aspnetcore.html#creating-an-mvc-client


https://www.cnblogs.com/stulzq/p/8119928.html

https://cloud.tencent.com/developer/article/1696517


https://www.cnblogs.com/linianhui/p/oidc-in-action-sso.html

https://img2020.cnblogs.com/blog/824291/202004/824291-20200406223103730-527609758.jpg


https://zhaobingwang.blog.csdn.net/article/details/103824965


http://www.identityserver.com.cn/

https://www.cnblogs.com/qtiger/p/14889151.html

https://www.jb51.net/article/183505.htm

https://www.cnblogs.com/fengchao1000/category/1327628.html

https://www.cnblogs.com/stulzq/category/1060023.html

## 高并发和高可用

* 分层
    * 逻辑分层
    * 物理部署

* 冗余
    * 集群
    * 备份 定期，冷热
    * 灾备

* 分隔（纵向）

* 异步
    * 共享内存
    * 消息队列

* 分布式架构

* 架构安全
    * 身份认证
    * 防机器人程序
    * 通信加密
    * 垃圾信息过滤

* 信息自动化

    * 自动化发布
    * 自动化代码管理
    * 自动化检测
    * 自动化测试

* 架构集群

    * 反向代理



## 服务网格



## Portainer


## sqldependency

## graphql

## zabbix



## vue.mixin

## vue.slot


