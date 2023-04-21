## 设计模式

GOF/Gang of Four/四位作者

设计模式主要是基于以下的面向对象设计原则。

* 对接口编程而不是对实现编程。
* 优先使用对象组合而不是继承。

## 分类

* 创建型模式```Creational Patterns```
    > 在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象。

    <!-- 单例，工厂，建造，原型 -->

    * 工厂方法模式```Factory Pattern```
    * 抽象工厂模式
    * [单例模式```Singleton Pattern```](https://www.cnblogs.com/thomerson/p/16795927.html)
    * 建造者模式
    * 原型模式


* 结构型模式```Structural Patterns```
    > 关注类和对象的组合

    <!-- 装适代门组享桥 -->
    
    * 适配器模式
    * 装饰器模式
    * 代理模式
    * 外观模式
    * 桥接模式
    * 组合模式
    * 享元模式
    
* 行为型模式
    > 关注对象之间的通信

    <!-- 策模观解
    责迭命中
    访备状态 -->

    * 策略模式
    * 模板方法模式
    * 观察者模式
    * 迭代子模式
    * 责任链模式
    * 命令模式
    * 备忘录模式
    * 状态模式
    * 访问者模式
    * 中介者模式
    * 解释器模式


## 设计模式的六大原则

1. 开闭原则```Open Close Principle```

2. 里氏代换原则```Liskov Substitution Principle```

3. 依赖倒转原则```Dependence Inversion Principle```
    * 高层模块不应该依赖低层模块，两者都应该依赖抽象
    * 抽象不应该依赖于实现，实现应该依赖于抽象

4. 接口隔离原则```Interface Segregation Principle```

5. 迪米特法则，又称最少知道原则```Demeter Principle```

6. 合成复用原则```Composite Reuse Principle```


## 一些相似的设计模式

1. 状态模式和策略模式

    * 策略模式是让用户指定更换的策略算法
    
    * 状态模式是状态在满足一定条件下的自动更换，用户无法指定状态，最多只能设置初始状态。 

    <!-- https://www.runoob.com/w3cnote/state-vs-strategy.html -->

2. 建造者和工厂模式

在工厂模式中，关注的是谁创建了这个产品

在建造者模式中，**这个产品会有多个复杂工序**，例如组装电脑，提供了一个builder类来管理这个组装过程


## 设计模式在dotnet中的应用

1. 责任链模式

asp.net core 中间件的设计，每个中间件根据需要处理请求，并且可以根据请求信息自己决定是否传递给下一个中间件

2. 建造者模式

    asp.net core 中的各种 Builder， ```HostBuilder```/```ConfigurationBuilder```

3. 工厂模式

    依赖注入框架中有着大量的工厂模式的代码，注册服务的时候我们可以通过一个工厂方法委托来获取服务实例，


4. 单例模式

    * 数据库连接池

        为什么要做池化，是因为新建连接很耗时，如果每次新任务来了，都新建连接，那对性能的影响实在太大。所以一般的做法是在一个应用内维护一个连接池，这样当任务进来时，如果有空闲连接，可以直接拿来用，省去了初始化的开销。所以用单例模式，正好可以实现一个应用内只有一个线程池的存在，所有需要连接的任务，都要从这个连接池来获取连接。

    * 配置文件访问

        如果不用单例的话，每次都要 new 对象，每次都要重新读一遍配置文件，很影响性能，如果用单例模式，则只需要读取一遍就好了。

    

5. 原型模式

dotnet 中有两个数据结构 ```Stack```/```Queue``` 这两个数据都实现了 ICloneable 接口，内部实现了深复制



```c#
// Stack 的 Clone 方法实现
public virtual Object Clone()
{
    Contract.Ensures(Contract.Result<Object>() != null);
 
    Stack s = new Stack(_size);
    s._size = _size;
    Array.Copy(_array, 0, s._array, 0, _size);
    s._version = _version;
    return s;
}

```

6. 享元模式

string intern(**字符串池**)，以及 ```Array.Empty<int>()```/```Array.Empty<string>()``` 等


7. 策略模式

ASP.NET Core授权

在使用基于策略的授权时，首先要定义授权策略，而授权策略本质上就是对Claims的一系列断言。
基于角色的授权和基于Scheme的授权，只是一种语法上的便捷，最终都会生成授权策略。

```c#

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        //options.AddPolicy("Administrator", policy => policy.RequireRole("administrator"));
        options.AddPolicy("Administrator", policy => policy.RequireClaim(ClaimTypes.Role, "administrator"));
        
        //options.AddPolicy("Founders", policy => policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}

[Authorize(Policy = "Administrator")]
public ActionResult<IEnumerable<string>> GetValueByAdminPolicy()
{
    return new string[] { "GetValueByAdminPolicy" };
}

```

8. 观察者模式

常使用事件(event)进行解耦，外部代码通过订阅事件来解耦，实现对内部状态的观察

9. 组合模式

WPF、WinForm 中都有控件的概念，这些控件的设计属于是组合模式的应用，所有的控件都会继承于某一个共同的基类, 使得单个对象和组合对象都可以看作是他们共同的基类对象

10. 迭代器模式

```c#

// 聚集抽象
public interface IEnumerable
{
    /// <summary>Returns an enumerator that iterates through a collection.</summary>
    /// <returns>An <see cref="T:System.Collections.IEnumerator" /> object that can be used to iterate through the collection.</returns>
    IEnumerator GetEnumerator();
}

// 迭代器抽象
public interface IEnumerator
{
    /// <summary>Advances the enumerator to the next element of the collection.</summary>
    /// <returns>
    /// <see langword="true" /> if the enumerator was successfully advanced to the next element; <see langword="false" /> if the enumerator has passed the end of the collection.</returns>
    /// <exception cref="T:System.InvalidOperationException">The collection was modified after the enumerator was created.</exception>
    bool MoveNext();

    /// <summary>Gets the element in the collection at the current position of the enumerator.</summary>
    /// <returns>The element in the collection at the current position of the enumerator.</returns>
    object Current { get; }

    /// <summary>Sets the enumerator to its initial position, which is before the first element in the collection.</summary>
    /// <exception cref="T:System.InvalidOperationException">The collection was modified after the enumerator was created.</exception>
    void Reset();
}


```