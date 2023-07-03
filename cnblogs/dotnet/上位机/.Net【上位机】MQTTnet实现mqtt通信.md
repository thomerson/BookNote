## MQTTnet

MQTTnet 是一个跨平台、高性能和开源的 MQTT 客户端库和服务端实现，是 .NET 平台上主流的 MQTT 实现之一

[Git地址](https://github.com/dotnet/MQTTnet)

## Net7实现mqtt

说明：客户端，服务端，发布者和订阅者

MQTT 服务端主要用于与多个客户端保持连接，并处理客户端的发布和订阅等逻辑。



一般很少直接从服务端发送消息给客户端（可以使用 ```mqttServer.Publish(appMsg);``` 直接发送消息），多数情况下服务端都是转发主题匹配的客户端消息，在系统中起到一个中介的作用。




## asp.net core继承mqtt

