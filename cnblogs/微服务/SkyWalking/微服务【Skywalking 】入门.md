## Skywalking

分布式系统的应用程序性能监视工具，专为微服务，云原生架构和基于容器（Docker，K8S,Mesos）架构而设计，

它是一款优秀的APM（Application Performance Management）工具，包括了分布式追踪，性能指标分析和服务依赖分析等


### 功能

* 多种监控手段。可以通过语言探针和 service mesh 获得监控是数据。

* 多个语言自动探针。包括 Java，.NET Core 和 Node.JS。

* 轻量高效。无需大数据平台，和大量的服务器资源。

* 模块化。UI、存储、集群管理都有多种机制可选。

* 支持告警。

* 优秀的可视化解决方案

### 架构

![架构](https://skywalking.apache.org/zh/2020-04-19-skywalking-quick-start/0081Kckwly1gkl533fk5xj31pc0s8h04.jpg)

* 探针/Agent

    Agent 运行在各个服务实例中，负责采集服务实例的 Trace 、Metrics 等数据，然后通过 gRPC 方式上报给 SkyWalking 后端


* 平台后端/OAP

    SkyWalking 的后端服务

    主要责任

    * 一个是负责接收 Agent 上报上来的 Trace、Metrics 等数据，交给 Analysis Core （涉及 SkyWalking OAP 中的多个模块）进行流式分析，最终将分析得到的结果写入持久化存储中。SkyWalking 可以使用 ElasticSearch、H2、MySQL 等作为其持久化存储，一般线上使用 ElasticSearch 集群作为其后端存储。

    * 负责响应 SkyWalking UI 界面发送来的查询请求，将前面持久化的数据查询出来，组成正确的响应结果返回给 UI 界面进行展示

* 用户界面

    SkyWalking 前后端进行分离，该 UI 界面负责将用户的查询操作封装为 GraphQL 请求提交给 OAP 后端触发后续的查询操作，待拿到查询结果之后会在前端负责展示。


