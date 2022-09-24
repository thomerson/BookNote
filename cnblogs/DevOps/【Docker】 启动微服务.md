## 启动docker服务

关机之后重新启动虚拟机发现docker命令不起作用，原因是因为没有**启动docker服务**，需要用```service docker start```启动docker服务

## 单个服务部署

1. visual studio 发布

2. 使用winSCP复制文件到linux对应的路径中

3. 生成dockerfile文件

```shell
FROM mcr.microsoft.com/dotnet/aspnet:6.0
WORKDIR /publish
EXPOSE 80
EXPOSE 443
COPY publish/ /publish
ENTRYPOINT ["dotnet","gatlin.demo.shopping.orderapi.dll"]
```

4. 生成镜像

```shell
docker build -t orderservicepublish .
```

5. run容器

```shell
docker run -p 5001:80 orderservicepublish
```


## 多个服务同时部署

* docker compose

1. 配置```yaml/yml```文件

```yml
version: '1'
services:
    productservice:
        image: productservicepublish
        ports:
         - 5001:80
    orderservice:
        image: orderservicepublish
        ports:
         - 5002:80
```

2. docker-compose启动所有服务

```shell
docker-compose up -d
``` 


## 一次性生成多个镜像

CI/CD

1. 自动发布文件

2. 自动生成镜像


