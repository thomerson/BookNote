## Helm

Kubernetes 的**软件包管理工具**


似于我们在 Ubuntu 中使用的apt、Centos中使用的yum 或者Python中的 pip 一样，能快速查找、下载和安装软件包。

* Tiller

    是 Helm 的**服务端**，部署在 Kubernetes 集群中

    Tiller 用于接收 Helm 的请求，并根据 Chart 生成 Kubernetes 的部署文件（ Helm 称为 Release ），然后提交给 Kubernetes 创建应用。Tiller 还提供了 Release 的升级、删除、回滚等一系列功能


* Chart

    Helm 的软件包，采用 TAR 格式。类似于 APT 的 DEB 包或者 YUM 的 RPM 包，其包含了一组定义 Kubernetes 资源相关的 YAML 文件。

* Repoistory

    Helm 的软件仓库，Repository 本质上是一个 Web 服务器，该服务器保存了一系列的 Chart 软件包以供用户下载，并且提供了一个该 Repository 的 Chart 包的清单文件以供查询。Helm 可以同时管理多个不同的 Repository。

* Release

    使用 helm install 命令在 Kubernetes 集群中部署的 Chart 称为 Release


### 安装

```shell
$ 下载 Helm 二进制文件
$ wget https://storage.googleapis.com/kubernetes-helm/helm-v2.9.1-linux-amd64.tar.gz
$ 解压缩
$ tar -zxvf helm-v2.9.1-linux-amd64.tar.gz
$ 复制 helm 二进制 到bin目录下
$cp linux-amd64/helm /usr/local/bin/
```


