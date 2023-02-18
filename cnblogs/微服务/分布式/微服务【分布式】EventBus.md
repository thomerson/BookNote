https://www.cnblogs.com/mq0036/p/17104130.html

## 事件总线

事件总线是对发布-订阅模式的一种实现，是一种集中式事件处理机制，允许不同的组件之间进行彼此通信而又不需要相互依赖，达到一种解耦的目的。

![事件总线](https://upload-images.jianshu.io/upload_images/2799767-6f44bdefa88a23a2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 发布订阅模式

发布订阅主要有两个角色
    
* 发布方（Publisher）：也称为被观察者，当状态改变时负责通知所有订阅者。
* 订阅方（Subscriber）：也称为观察者，订阅事件并对接收到的事件进行处理。

两种实现方式

* 遍历通知：由Publisher维护一个订阅者列表，当状态改变时循环遍历列表通知订阅者

* 委托通知：由Publisher定义事件委托，Subscriber实现委托

![发布订阅](https://upload-images.jianshu.io/upload_images/2799767-8a17f6e834278167.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 事件总线的主要功能

* 事件总线维护一个事件源与事件处理的映射字典
* 通过单例模式，确保事件总线的唯一入口
* 利用反射完成事件源与事件处理的初始化绑定
* 提供统一的事件注册、取消注册和触发接口


### 解决方案

* [CAP](https://www.cnblogs.com/savorboard/p/cap.html)

