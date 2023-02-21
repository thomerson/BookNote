## gRPC 和 RESTful

|比较|RPC|RESTful|
|---|---|---|
|数据格式|Protobuf|JSON |
|通信协议|http/2|http/1.1|
|浏览器支持|有限，需要转换|通用支持|


### Protobuf 与 JSON

REST 使用 JSON 格式接收消息。虽然我们可以接收 XML、原始二进制格式等格式的消息，但最佳实践和教程使 JSON 成为规范，而且由于 JSON 灵活、高效、平台中立且与语言无关

gRPC 使用 Protobuf 消息格式以消息二进制格式发送请求和接收响应。

JSON 在系统之间传输时速度较慢，可读性好。Protobuf 消息传递速度更快。


### HTTP/1.1 与 HTTP/2


REST 在通信以及发送请求和接收响应中使用 HTTP/1.1。gRPC 使用 HTTP/2，这对于进程间通信来说甚至更快。