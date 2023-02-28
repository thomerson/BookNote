## 安装

### 准备

1. Elasticsearch 如果用数据存储用es的话

### windows安装

1. 下载SkyWalking

    [下载地址](https://skywalking.apache.org/downloads/)


### docker安装


2. 搭建SkyWalking OAP服务

oap服务需要关联ES

```shell
docker run --name skywalking-oap \
--restart always \
-p 11800:11800 -p 12800:12800 -d \
-e TZ=Asia/Shanghai \
-e SW_ES_USER= \
-e SW_ES_PASSWORD= \
-e SW_STORAGE=elasticsearch \
-e SW_STORAGE_ES_CLUSTER_NODES=192.168.101.10:9200 \
-v /etc/localtime:/etc/localtime:ro \
apache/skywalking-oap-server:8.9.1
```

3. 搭建一个 SkyWalking UI 服务

skywalking-ui界面需要关联oap服务

```shell
docker run -d \
--name skywalking-ui \
--restart always \
-p 8080:8080 \
--link skywalking-oap:skywalking-oap \
-e TZ=Asia/Shanghai \
-e SW_OAP_ADDRESS=http://skywalking-oap:12800 \
-v /etc/localtime:/etc/localtime:ro \
apache/skywalking-ui:8.9.1
```

打开8080查看skywalking界面

