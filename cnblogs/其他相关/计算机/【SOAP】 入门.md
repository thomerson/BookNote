## SOAP
> Simple Object Access Protocol/简易对象访问协议

基于**XML** 的简易协议

可使应用程序在 HTTP 之上进行信息交换

## 语法

一条 SOAP 消息就是一个普通的 XML 文档，包含下列元素

* Envelope：**必需**，把此 XML 文档标识为一条 SOAP 消息
    * encodingStyle属性：定义在文档中使用的数据类型 
* Header：可选
    * encodingStyle属性
    * actor
    * mustUnderstand 指示处理此头部的接收者必须认可此元素
* Body：**必需**,包含所有的调用和响应信息
* Fault: 可选，提供有关在处理此消息所发生错误的信息

```xml
<?xml version="1.0"?>
<soap:Envelope
xmlns:soap="http://www.w3.org/2001/12/soap-envelope"
soap:encodingStyle="http://www.w3.org/2001/12/soap-encoding">

<soap:Header>
...
</soap:Header>

<soap:Body>
...
  <soap:Fault>
  ...
  </soap:Fault>
</soap:Body>

</soap:Envelope>
```