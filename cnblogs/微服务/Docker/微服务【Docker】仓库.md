## Docker Hub

Docker官方维护了一个公共仓库Docker Hub

大部分需求都可以通过在Docker Hub中直接下载镜像来使用

```shell
# 查找镜像
docker search tomcat

# 拉取镜像
docker pull tomcat

# 推送
docker push 

```

## 构建私有仓库

官网工具```docker-registry```可以用于构建私有的镜像仓库

```shell
# 运行docker-registry
docker run --name registry -d  -p 5000:5000 --restart=always  -v /opt/data/registry:/var/lib/registry registry
```

1. daemon.json配置

2. tag设置版本

3. push向仓库推送镜像

4. pull下载