## GraphQL


 API 的查询语言

 
### 特点


* 只请求必要的数据字段

    例如user 中有 name, age, sex 等，可以只取得需要的name字段。

* 只用一个请求获取多个资源

可以通过一次请求就获取你应用所需的所有数据。

* 描述所有可能类型，便于维护，根据需求平滑演进，添加或隐藏字段；



## GraphQL与restful对比

* restful 一个接口只能返回一个资源，GraphQL一次可以获取多个资源。

* restful 用不同 url 来区分资源，GraphQL 用类型区分资源。

### 不流行的原因

利好主要是在于前端的开发效率，但落地却需要服务端的全力配合。


<!-- TODO -->

<!-- https://blog.csdn.net/xplan5/article/details/108716321 -->