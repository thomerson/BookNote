## 【InfluxDB】安装

1. docker拉取镜像

```powershell
docker pull influxdb
```

2. 启动

```powershell
docker run -p 8083:8083 -p 8086:8086 --name influxdb -td influxdb:latest
```

3. 登录配置

```http://127.0.0.1:8086/```

设置用户名密码

![image](https://img2023.cnblogs.com/blog/999484/202308/999484-20230806141444618-1324102923.png)


4. 创建bucket，存储数据

![image](https://img2023.cnblogs.com/blog/999484/202308/999484-20230806141852268-1269437437.png)
