## 资源所有者密码模式

允许一个客户端发送用户名和密码到令牌服务并获得一个表示该用户的访问令牌

在客户端凭证模式上进行修改

## 实现

* [IDS4](#identityserver)

    提供认证的服务

* [API](#api)

    需要做认证的API接口

* [Client](#client)

    请求调用api的客户端


### identityserver

1. 添加```Users```


```c#

public static IEnumerable<TestUser> Users => new List<TestUser> {
    new TestUser
    {
        SubjectId="1",
        Username="爱丽丝",
        Password="password"
    },
    new TestUser
    {
        SubjectId="2",
        Username="博德",
        Password="password"
    }
};
```

2. 添加Client，设置IDs4模式

```c#
public static IEnumerable<Client> Clients => new List<Client> {
    new Client(){
        ClientId =  "ClientA",
        AllowedGrantTypes = GrantTypes.ClientCredentials, // 客户端模式
        AllowedScopes = new List<string> { "api1" },
        ClientSecrets  = { new Secret("secret".Sha256())}
    },
    new Client(){
        ClientId = "ClientB",
        AllowedGrantTypes = GrantTypes.ResourceOwnerPassword,  // 资源所有者模式
        AllowedScopes = new List<string> { "api1" },
        ClientSecrets  = { new Secret("secret".Sha256())}
    }
};

```

3. 将测试用户注册到IDS4

```c#
public void ConfigureServices(IServiceCollection services)
{
    // 使用内存存储，密钥，客户端和资源来配置身份服务器。
    services.AddIdentityServer()
        .AddTemporarySigningCredential()
        .AddInMemoryApiResources(Config.GetApiResources())
        .AddInMemoryClients(Config.GetClients())
        .AddTestUsers(Config.GetUsers());  //users
}

```


### API

和客户端模式一样，不用修改


### Client

```c#

tatic void ResourceOwnerPasswordRequest()
{
    var username = "爱丽丝";
    var password = "password";

    var client = new HttpClient();

    var result = client.GetDiscoveryDocumentAsync("http://localhost:5600/").Result;

    if (result.IsError)
    {
        Console.WriteLine($"result:{result.Error}");
        //Console.ReadLine();
    }

    // ⭐ 这里请求方法不一样
    var resultWithToken = client.RequestPasswordTokenAsync(new PasswordTokenRequest
    {
        Address = result.TokenEndpoint,
        ClientId = "ClientB",
        ClientSecret = "secret",
        Scope = "api1",
        UserName = username,
        Password = password
    }).Result;

    if (resultWithToken.IsError)
    {
        Console.WriteLine($"resultWithToken:{resultWithToken.Error}");
    }

    Console.WriteLine($"token:{resultWithToken.Json}");

    //调用认证api
    var apiClient = new HttpClient();
    apiClient.SetBearerToken(resultWithToken.AccessToken);

    var response = apiClient.GetAsync("http://localhost:31773/identity").Result;
    if (!response.IsSuccessStatusCode)
    {
        Console.WriteLine($"response:{response.StatusCode}");
    }
    else
    {
        var content = response.Content.ReadAsStringAsync().Result;
        Console.WriteLine($"content:{content}");
    }

}

```


### 客户端模式和资源所有者密码模式

解析请求的BearerToken，资源所有者密码模式多个一个重要的```sub```这个申明


