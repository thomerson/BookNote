## kubernetes

**基于容器的集群管理平台**

Kubernetes 这个单词来自于希腊语，含义是**舵手**或**领航员**。K8S是它的缩写，用“8”字替代了“ubernete”这8个字符


Google开源

## K8S集群

![K8S集群](https://pic4.zhimg.com/80/v2-466804fc47bd2e939e0413d9c32170af_720w.webp)


* 一个Master节点（主节点）

    负责管理和控制

* 一群Node节点（计算节点）

    工作负载节点，里面是具体的容器

### Master节点

![Master节点](https://pic2.zhimg.com/80/v2-7fa63b292368c8f21bd4582861a6983d_720w.webp)


* API Server

    整个系统的对外接口，供客户端和其它组件调用，相当于“营业厅”

* Scheduler

    负责对集群内部的资源进行调度，相当于“调度室”。

* Controller manager

    负责管理控制器，相当于“大总管”


* etcd

    是一个分布式的一个存储系统，API Server 中所需要的这些原信息都被放置在 etcd 中，etcd 本身是一个高可用系统，通过 etcd 保证整个 Kubernetes 的 Master 组件的高可用性。

### Node节点

![node节点](https://pic4.zhimg.com/80/v2-8cb338cd8923fa0e6857f45facc8f00f_720w.webp)

* Docker

    创建容器

* kubelet

    主要负责监视指派到它所在Node上的Pod，包括创建、修改、监控、删除等。

* kube-proxy

    主要负责为Pod对象提供代理

* Fluentd

    主要负责日志收集、存储与查询

* kube-dns（可选）

* Pod

    是 Kubernetes 的一个最小调度以及资源单元。

    一个 Pod 简单来说是对一组容器的抽象，它里面会包含一个或多个容器


<!-- https://kubernetes.io/zh-cn/docs/home/ -->

<!-- https://www.infoq.cn/article/KNMAVdo3jXs3qPKqTZBw -->


<!-- 
https://mp.weixin.qq.com/s?__biz=MzI1NzI5NDM4Mw==&mid=2247483903&idx=1&sn=acfe1eae0a0e25d5338a43f7b9eb5732&chksm=ea18e8bfdd6f61a9428784e5985c6706f5e99f25e1ee1fba4d9efde1b31d97aa09d34ba9d4dc&token=2123136639&lang=zh_CN&scene=21#wechat_redirect -->