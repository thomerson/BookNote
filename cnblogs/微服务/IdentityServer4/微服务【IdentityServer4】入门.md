## IdentityServer4

ASP.NET Core 2.X

一款基于**OpenID Connect**和**OAuth 2.0** 认证框架


## 概念

1. Authentication && Authorization

* 认证/Authentication

通过认证以确定用户身份

* 授权/Authorization

给用户分配可权限，以确定用户可访问的资源范围。授权的前提是要确认用户身份，即先认证，再授权。

2. OpenID && OpenID Connect

* OpenID：由OpenID基金会维护的第三方认证规范，存在如下缺点：
    * 以URI为用户唯一标识，用户难以记忆
    * 第三方应用必须是网站，没有提供API，不支持移动应用
    * 不支持健壮的加密和签名
* OpenID Connect: 基于OAuth2.0实现的用户认证规范。相对OpenID提供了如下增强特性

    * 提供可扩展性，运行人们通过任何OpenID Connect Provider进行身份验证，而不是仅限于Google、Facebook等主流IDP。
    * 电子邮件作为用户标识，便于用户记忆。
    * 允许客户端动态注册，减轻管理员显示注册设备和网站的工作量。

https://segmentfault.com/a/1190000023938486

3. 身份令牌 && 访问令牌

* 身份令牌：标识某个用户（Called the sub aka subject claim）的主身份信息，和该用户的认证时间和认证方式
* 访问令牌：API通过这些令牌信息来授予客户端的数据访问权限

## IdentityServer4特点

* 认证服务

    集中式的登录逻辑和工作流控制

* 单点登录登出(SSO)

* API访问控制

* 联合网关

    支持来自Azure Active Directory, Google, Facebook这些知名应用的身份认证，可以不必关心连接到这些应用的细节就可以保护你的应用。

* 专注于定制


## 模式

* Client Credentials/客户端凭证模式

    是IdentityServer4中控制API访问的基本模式，我们通常通过定义一个API和访问客户端，客户端通过客户端ID请求Identity Server以获取访问的token

* 用户密码模式

    直接通过用户名和密码获得授权，这种适用于高度信任的应用，因为需要输入用户名和密码，安全泄露风险高

## 参考

* [identityserver4中文文档](http://www.identityserver.com.cn/)
