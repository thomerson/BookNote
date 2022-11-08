[微服务](https://img2020.cnblogs.com/blog/824291/202004/824291-20200406223103730-527609758.jpg)

## 6大组件
* 服务描述
* 注册中心
* 服务架构
* 服务监控
* 服务跟踪
* 服务治理

### 服务监控

Zabbix


## 服务发现/注册

* consul

* zookeeper

## 网关/API Gateway

API网关是一个服务器，是系统的唯一入口。

核心要点是，所有的客户端和消费端都通过统一的网关接入微服务，在网关层处理所有的非业务功能。

还提供了一些更高级的功能，例如：身份验证、监控、负载均衡、缓存、多协议支持、限流、熔断等等。

### 网关的设计要素

* 限流：实现微服务访问流量计算，基于流量计算分析进行限流，可以定义多种限流规则。
* 缓存：数据缓存。
* 日志：日志记录。
* 监控：记录请求响应数据，api耗时分析，性能监控。
* 鉴权：权限身份认证。
* 灰度：线上灰度部署，可以减小风险。
* 路由：路由是API网关很核心的模块功能，此模块实现根据请求，锁定目标微服务并将请求进行转发。

### Ocelot

1. [微服务【Ocelot】入门]()
2. [微服务【Ocelot】配置]()
3. [微服务【Ocelot】请求聚合]()
4. [微服务【Ocelot】集成Polly实现QoS]()
5. [微服务【Ocelot】集成Cache]
6. [微服务【Ocelot】集成Cache使用CacheManager]
7. [微服务【Ocelot】集成Cache使用Redis]
8. [微服务【Ocelot】集成Cache自定义]
9. [微服务【Ocelot】集成IdentityServer4实现权限验证]
10. [微服务【Ocelot】集成Consul]



## 反向代理

Nginx

## 系统日志


### ExceptionLess

1. [微服务【Exceptionless】入门]

### ElasticSearch

1. [微服务【ElasticSearch】入门]
2. [微服务【Kibana】入门]

## 自动化集成

Jenkins

GitLab

## 分布式监控系统

* 链路追踪


### Skywalking
[微服务【Skywalking 】入门]()

## 容器部署

Docker

### 弹性收缩

K8S


## 配置中心

Apollo


## 授权中心

IdentityServer4


## 数据一致性

CAP


## 服务治理

Polly


## 服务间通信

gRPC

REST








https://www.cnblogs.com/xhznl/p/13071260.html

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

* 