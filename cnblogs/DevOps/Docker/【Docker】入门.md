## Docker

1. what：**容器技术**

2. why: 精简的linux系统，用来部署微服务

3. 优点

    * 单机部署缺陷
        > 原因：部署耦合，在同一个操作系统上
        * 端口冲突
        * 环境冲突

    * 虚拟机部署缺陷
        * 耗资源问题
        * 使用困难：配置复杂，维护困难
    
    * 容器
        * 复用操作系统的资源

4. 版本
    
    * CE/community Edition/社区版 免费
    * EE/Enterprise Edition/企业版 收费




开源的应用容器引擎

基于Go 语言


### 容器

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。



## 概念

* 镜像/Image⭐

    相当于是一个 root 文件系统
    
    * 精简Linux系统
    * 应用+精简Linux系统
    * 应用+运行环境+精简Linux系统
 

**模板**

* 容器/Container⭐

**独立运行的应用**

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

* 仓库/Repository⭐
一个代码控制中心，用来**保存镜像**

* Docker 客户端/Client

**操作docker host**

Docker 客户端通过命令行或者其他工具使用 Docker SDK 与 Docker 的守护进程通信。

* Docker 主机/Host

**管理镜像和容器**

一个物理或者虚拟的机器用于执行 Docker 守护进程和容器

* Docker Machine

Docker Machine是一个**简化Docker安装**的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。



* dockerfile⭐

是一个用来**构建镜像**的文本文件，文本内容包含了一条条构建镜像所需的指令和说明

container registry

## 命令


## 浏览器请求过程


        浏览器->linux->docker->容器(浏览器访问的端口号)->微服务(端口80)                                 


## Docker Compose

**容器编排**

运行**多容器** Docker 应用程序的工具

一次性运行多个微服务

* 缺点
    1. 只能在当前主机上使用，**不能跨主机操作**


## Docker Swarm

**Docker 集群**

* 解决问题

    * 提高并发量

    * 解决单点故障
* 实现手段
    * swarm
    * k8s


### 节点Node

* manager
    * 集群配置
    * 容器服务管理
    * 负载均衡
    * 集群其他配置

* worker

    * 提供容器服务

### 通过service/Task/stack配置集群

* service 一个docker容器同时部署在不同节点上的集合

* task 启动不同节点上的容器的任务

* stack 多个service的集合


### 启动swarm集群

* init
    初始化集群，创建管理节点

* join
    将其他docker节点加到集群，创建工作节点，并被管理节点管理

### swarm中部署service

* docker service 


## volums


## 安装

centOS安装命令

```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

也可以使用国内 daocloud 一键安装命令：

```shell
curl -sSL https://get.daocloud.io/docker | sh
```
