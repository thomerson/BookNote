## Prometheus

开源**监控报警系统**和**时序列数据库**(TSDB)

使用Go语言开发

### 特点

* 多维度数据模型。
* 灵活的查询语言。
* 不依赖分布式存储，单个服务器节点是自主的。
* 通过基于HTTP的pull方式采集时序数据。
* 可以通过中间网关进行时序列数据推送。
* 通过服务发现或者静态配置来发现目标服务对象。
* 支持多种多样的图表和界面展示，比如Grafana等

Prometheus 并没有采用 JSON 的数据格式，而是采用 text/plain 纯文本的方式

[官网](https://prometheus.io/)

### 基本原理

通过HTTP协议周期性抓取被监控组件的状态，任意组件只要提供对应的HTTP接口就可以接入监控。

### 架构


![架构](https://pic1.zhimg.com/80/v2-6c625d85a52daed32891c13bc4b00710_720w.webp)

* Server 

    主要负责数据采集和存储，提供PromQL查询语言的支持。

* web UI

* Alertmanager 

    警告管理器，用来进行报警。

* Push Gateway 

    支持临时性Job主动推送指标的中间网关。



### 数据持久化

* 本地存储

    通过 Prometheus 自带的 TSDB（时序数据库），将数据保存到本地磁盘，为了性能考虑，建议使用 SSD

* 远端存储

    适用于大量历史监控数据的存储和查询

    适配器实现 Prometheus 存储的 remote write 和 remote read 接口，并把数据转化为远端存储支持的数据格式。目前，远端存储主要包括 OpenTSDB、InfluxDB、Elasticsearch、M3db、Kafka 等，其中 M3db 是目前非常受欢迎的后端存储。


## Grafana


### 对比Zabbix

Zabbix适合用于虚拟机、物理机的监控，因为每个监控指标是以 IP 地址作为标识进行区分的。

而Prometheus的监控指标是由多个 label 组成，IP地址并不是唯一的区分指标，Prometheus 强大在可以支持自动发现规则，因此适合于容器环境。


从自定义监控项角度而言，Prometheus 开发难度较大，zabbix配合shell脚本更加方便。Prometheus在监控虚拟机上业务时，可能需要安装多个 exporter，而zabbix只需要安装一个 Agent。


Prometheus 采用拉数据方式，即使采用的是push-gateway，prometheus也是从push-gateway拉取数据。而Zabbix可以推可以拉。

<!-- TODO -->
<!-- https://zhuanlan.zhihu.com/p/267966193 -->