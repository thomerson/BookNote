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

* 容器/Container⭐

镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。

* 仓库/Repository⭐
一个代码控制中心，用来保存镜像

* Docker 客户端/Client
Docker 客户端通过命令行或者其他工具使用 Docker SDK 与 Docker 的守护进程通信。

* Docker 主机/Host

一个物理或者虚拟的机器用于执行 Docker 守护进程和容器

* Docker Machine

Docker Machine是一个简化Docker安装的命令行工具，通过一个简单的命令行即可在相应的平台上安装Docker，比如VirtualBox、 Digital Ocean、Microsoft Azure。




docker image

* dockerfile⭐

是一个用来**构建镜像**的文本文件，文本内容包含了一条条构建镜像所需的指令和说明

container registry

## 命令


## 浏览器请求过程


        浏览器->linux->docker->容器(浏览器访问的端口号)->微服务(端口80)                                 


## Docker Compose

运行**多容器** Docker 应用程序的工具

一次性运行多个微服务

