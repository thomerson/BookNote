## gRPC

Remote Procedure Calls

google 开发

语言中立、平台中立、开源

gRPC是一个高性能、通用的开源RPC框架，基于底层HTTP/2协议标准和协议层Protobuf序列化协议开发，支持众多的开发语言。

gRPC 也是基于以下理念：定义一个服务，指定其能够被远程调用的方法（包含参数和返回类型）。在服务端实现这个接口，并运行一个 gRPC服务器来处理客户端调用。在客户端拥有一个存根能够像服务端一样的方法。


双向流、流控、头部压缩、单 TCP 连接上的多复用请求等特

## protocol buffers


## 通信方式

### Simple RPC/简单rpc

> 一个请求对象对应一个返回对象


```
rpc simpleHello(Person) returns (Result) {}
```

### Server-side streaming RPC

> 服务端流式rpc

> 一个请求对象，服务端可以传回多个结果对象

```
rpc serverStreamHello(Person) returns (stream Result) {}
```

### Client-side streaming RPC

> 客户端流式rpc 

> 客户端传入多个请求对象，服务端返回一个响应结果


```
rpc clientStreamHello(stream Person) returns (Result) {}
```

### Bidirectional streaming RPC

> 双向流式rpc

> 结合客户端流式rpc和服务端流式rpc，可以传入多个对象，返回多个响应对象

```
rpc biStreamHello(stream Person) returns (stream Result) {}
```




