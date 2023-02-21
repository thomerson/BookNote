## 集成到dotnetCore


### server

visual studio可以通过选择gRPC项目快速生成gRPC服务


1. 安装Nuget包

```powershell
install-package Grpc.AspNetCore
```

注意版本号，依赖中有```Google.Protobuf```


2. 添加proto文件

**注意格式**，否则编译不成功

这里service一般以I开头，表示接口，需要在项目中继承实现

```proto
syntax = "proto3";

option csharp_namespace = "Demo.gRPCServer.ProductService";

package ProductService;

// 定义服务名称
service ProductService{
	rpc GetProducts(ProductRequest) returns (ProductResponse);
}

// 定义入参
message ProductRequest{
	int32 id = 1;
	string name = 2;
}

// 定义出参
message ProductResponse{
	int32 status = 1;
	string message = 2;
}
```

3. 在工程文件中添加```Protobuf```后重新编译

```xml
	<ItemGroup>
		<Protobuf Include="Protos\Product.proto" GrpcServices="Server"></Protobuf>
	</ItemGroup>
```

4. ```ConfigureService```添加Grpc

```c#
services.AddGrpc();
```

5. ```Configure```暴露服务

```c#

app.UseEndpoints(endpoints =>
{
    //endpoints.MapGet("/", async context =>
    //{
    //    await context.Response.WriteAsync("Hello World!");
    //});

    endpoints.MapGrpcService<ProductServiceImpl>(); //暴露服务
});

```

6. 在program中修改```http```为```Http2```协议

```c#
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
            webBuilder.UseKestrel(options =>
            {
                options.ConfigureEndpointDefaults(options =>
                {
                    // Http2
                    options.Protocols = Microsoft.AspNetCore.Server.Kestrel.Core.HttpProtocols.Http2; 
                });
            });
        });

```

7. 使用cmd命令```dotnet run```启动服务



### Client


1. 安装Nuget包

```powershell
install-package Grpc.Net.Client
install-package Google.Protobuf
install-package Grpc.Tools
```

2. 添加proto文件

```proto
syntax = "proto3";

option csharp_namespace = "Demo.gRPCaggregate.Client";

package ProductService;

// 定义服务名称
service ProductService{
	rpc GetProducts(ProductRequest) returns (ProductResponse);
}

// 定义入参
message ProductRequest{
	int32 id = 1;
	string name = 2;
}

// 定义出参
message ProductResponse{
	int32 status = 1;
	string message = 2;
}

```


3. 在工程文件中添加```Protobuf```后重新编译

```xml
	<ItemGroup>
		<Protobuf Include="Protos\Product.proto" GrpcServices="Client"></Protobuf>
	</ItemGroup>
```


4. 查询商品微服务

```c#
 // 1. 建立连接
using (var grpcChannel = GrpcChannel.ForAddress("https://localhost:44350/"))
{
    // 2. 创建client
    var client = new ProductService.ProductServiceClient(grpcChannel);

    // 3. 开始调用
    var response = client.GetProducts(new ProductRequest()
    {
        Id = 1,
        Name = ""
    });

    // 业务
    Console.WriteLine($"response:{response.Status},{response.Message}");
}

```

5. 通过在service中注册实现依赖注入

先安装nuget包

```powershell
install-package Grpc.Net.ClientFactory
```


在controller中使用client

```c#
namespace Demo.gRPCaggregate.Client.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class AggregateController : ControllerBase
    {
        private readonly ProductService.ProductServiceClient _productServiceClient;
        public AggregateController(ProductService.ProductServiceClient productServiceClient)
        {
            this._productServiceClient = productServiceClient;
        }

        [HttpGet]
        public JsonResult Get()
        {
            // 其他服务同样使用
            var response = _productServiceClient.GetProducts(new ProductRequest()
            {
                Id = 1,
                Name = ""
            });

            Console.WriteLine($"response:{response.Status},{response.Message}");

            return new JsonResult(response);
        }
    }
}


```

## 其他优化

1. 提取proto到单独项目中

```xml
	<ItemGroup>
		<Protobuf Include="..\Demo.gRPCService.ProductProto\Protos\IProduct.proto" GrpcServices="Server" />
	</ItemGroup>
```

注意这个地方的引用地址

2. ```proto```中定义集合

```proto
// 定义服务名称
service IProductService{
	rpc GetProductById(ProductRequest) returns (ProductsResponse);

	rpc GetProducts(ProductRequest) returns (ProductResponse);
}

// 定义出参
message ProductResponse{
	int32 status = 1;
	string message = 2;
}

// 出参集合
message ProductsResponse{
	repeated ProductResponse product = 1;
}

```

3. 流式传输

优点：大数据量的情况下防止内存溢出

