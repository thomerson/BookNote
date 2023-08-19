## Mosquitto

1. docker安装

```powershell
docker pull eclipse-mosquitto
```

2. 启动docker容器

```powershell
docker run -itd --name mosquitto -p 1883:1883 -p 9001:9001 -v /mosquitto:/mosquitto eclipse-mosquitto
```

<!-- https://www.cnblogs.com/qumogu/p/16007609.html -->