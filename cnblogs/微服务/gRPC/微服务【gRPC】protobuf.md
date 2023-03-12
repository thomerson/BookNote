## protobuf

Protobuf是由Google开发的二进制格式，用于在不同服务之间序列化数据。是一种IDL（interface description language）语言

比json快六倍

## 和json比较

* 体积小

    * 无需分隔符
    * 空字段省略
    * tag二进制表示

* 编解码快
<!-- 

https://studygolang.com/articles/17118?fr=sidebar

https://blog.csdn.net/hailang2ll/article/details/104471141/


https://www.cnblogs.com/lianshuiwuyi/p/12221913.html##%E7%BC%96%E8%AF%91%E7%94%9F%E6%88%90

## protocol buffers

 Google 开源的一套成熟的结构数据序列化机制，类似JSON。
与JSON相比，具有更大的压缩比，更小的传输体积，更快的传输速度。

Protocol Buffer和JSON相比的特点如下：

### 优点

* 字段被编号，新添加的字段不影响老结构。解决了向后兼容问题。

* 自动化生成代码，简单易用（用protoc工具）。
* 二进制消息，效率高，性能高。

### 缺点
* 二进制格式，可读性差（抓包dump后的数据很难看懂）
* 对象冗余，字段很多，生成的类较大，占用空间。
* 默认不具备动态特性（可以通过动态定义生成消息类型或者动态编译支持）

## 语言版本

ProtoBuf 有两个语言版本：v2 与 v3，截止目前在使用 v3 的时候，需要在 *.proto 文件首行中明文标识syntax="proto3";

否则默认为 syntax="proto2"; 推荐使用最新的syntax = "proto3";语法。 -->

