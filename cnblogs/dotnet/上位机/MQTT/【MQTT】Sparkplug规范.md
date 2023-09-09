## Sparkplug规范

Sparkplug提供开放、免费的规范，解决边缘网关或内置MQTT的终端设备，与MQTT应用通过MQTT网络(Infrastructure)进行双向通讯问题。

Sparkplug规范力图实现如下三个目标

1. 定义一套MQTT主题名
2. 定义MQTT状态管理
3. 定义MQTT负载


## EoN 

```Edge of Network```/边缘网络

边缘节点负责与现有传统设备(PLC、RTU、流量计、传感器等)、本地数字I/O、逻辑内部过程变量(PV)的本地协议接口

## 主题规范

所有采用Sparkplug 规范的MQTT客户端需遵循如下主题名结构

```namespace/group_id/message_type/edge_node_id/[device_id]```

1. ```namespace```
    
    定义了剩余元素的结构和相关负载数据的编码
    
2. ```group_id```
    
    提供对MQTT 边缘节点的逻辑分组
    
3. ```message_type```⭐
    
    说明消息的负载如何处理。注意，负载的编码取决于**namespace**元素所指的Sparkplug版本
    
    - NBIRTH – MQTT 边缘节点上线
    - NDEATH – MQTT 边缘节点下线
    - DBIRTH – 设备上线
    - DDEATH – 设备下线
    - NDATA – 节点数据(从节点读数据)
    - DDATA – 设备数据(从设备读数据)
    - NCMD – 节点命令(向节点写数据)
    - DCMD – 设备命令(向设备写数据)
    - STATE – 主应用状态

4. ```edge_node_id```
    
    **唯一标识**MQTT 边缘节点
    
5. ```device_id```
    
    标识一台关联到MQTT 边缘节点的(物理的或逻辑的)设备。**device_id**元素是可选的，一些消息直接源于或发送到**edge_node_id**，不需要**device_id**

### 会话管理

系统中主应用实例有权向节点和设备发送命令，所有边缘节点需要与主应用连接到相同MQTT服务器，否则节点就要转向另一服务器，这两项是任务关键型SCADA系统的已知需求。

主应用MQTT客户端和其他客户端**唯一**区别是非主客户端**不**发布上线/下线消息