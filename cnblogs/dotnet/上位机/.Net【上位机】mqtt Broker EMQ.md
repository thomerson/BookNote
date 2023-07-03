## EMQ


EMQ (Erlang/Enterprise/Elastic MQTT Broker) 是基于 Erlang/OTP 平台开发的开源物联网 MQTT 消息服务器


## docker安装emqx

```powershell
docker pull emqx/emqx:4.2.5
```
启动docker容器

```powershell
docker run -d --name emqx -p 1883:1883 -p 8883:8883 -p 8084:8084 -p 18083:18083 emqx/emqx:4.2.5
```


## Dashboard 控制台


地址: http://127.0.0.1:18083/

默认登录用户：admin       

密码：public


## MQTT调试工具

1. MQTTBox

[官网](http://workswithweb.com/mqttbox.html)似乎打不开，[GitHub](https://github.com/workswithweb/MQTTBox)上也没有release版本



2. MQTT.fx

版本：**mqttfx_1.7.1**

最新的版本需要**license**



