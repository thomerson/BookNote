## Portainer

官网：[https://www.portainer.io/](https://www.portainer.io/)

仓库地址：[https://hub.docker.com/r/portainer/](https://hub.docker.com/r/portainer/)

一个轻量级的管理 UI ,管理不同的 Docker 环境(Docker 主机或 Swarm 群集)


* 支持容器管理、镜像管理(导入、导出)。
* 轻量级，消耗资源少。
* 基于docker api，安全性高，可指定docker api端口，支持TLS证书认证。
* 支持权限分配。
* 支持集群。
* github上目前持续维护更新。


⭐开源版本的话只支持单机操作， 有一定的限制

### docker安装

1. 下载镜像

```shell
docker pull portainer/portainer
```


2. 运行容器

```shell
docker run -p 9000:9000 -p 8000:8000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v /mydata/portainer/data:/data -d portainer/portainer
```


3. 创建账号

    访问地址```[ip]:9000/```


4. 选择docker环境

    * 本地⭐

    * remote

        集群

        将 Portainer 服务安装启动在 Swarm 的管理节点上，并且首次设置 Endpoint URL 时设置管理节点的 URL

    * agent

    * azure

    ![docker环境](https://pic4.zhimg.com/80/v2-aa3d2a40980cbab83e4941e48d033fb7_720w.webp)



### 一览


* Dashboard
    
    打开Dashboard菜单可以看到Docker环境的概览信息，比如运行了几个容器，有多少个镜像等；

* App Templates

    创建容器的模板

    ![app-templates](https://pic1.zhimg.com/80/v2-a65eb19fa8a460e24fd92ffd40df754c_720w.webp)

* Containers

    容器操作，运行、暂停、删除

    * 日志

        ```docker logs```docker运行日志

    * Inspect

        查看容器信息，比如IP地址

    * Stats

        查看容器的内存、CPU及网络的使用情况


    * Console

        进入到容器中去执行命令

* Images

    查看所有的本地镜像，对镜像进行管理

* Networks

    查看Docker环境中的网络情况

* Users

    创建Portainer的用户，并给他们赋予相应的角色

* Registries

    配置自己的镜像仓库，这样在拉取镜像的时候，就可以选择从自己的镜像仓库拉取了



