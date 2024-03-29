 ## 领域驱动设计

```DDD```/```Domain-driven design```

一种解决业务复杂性的设计思想

前提是：

1. 把项目的主要重点放在核心领域（core domain）和域逻辑

2. 把复杂的设计放在有界域（bounded context）的模型上

3. 发起一个创造性的合作之间的技术和域界专家以迭代地完善的概念模式，解决特定领域的问题


领域驱动设计是一种由域模型来驱动着系统设计的思想，不是通过**存储数据词典**(DB表字段、ES Mapper字段等等)来驱动系统设计⭐

### DDD中的模型


模型（Model）承载着业务的属性和具体的行为，是业务表达的方式、是DDD的内核。是一个类中有属性、属性有Get/Set方法，并且**业务的行为（Action）操作也是在模型类**中（充血模型）即做业务逻辑处理，又做数据传输对象

* Entity/ 实体model
    * 有特定的标识，标识着这个Model在系统中全局唯一
    * 内部值可以是变化的，可能存在生命周期 (比如订单对象，状态值是连续变化的)
    * 有状态的Value Object

* Value Object /值对象
    * 内部值是不变的，不存在生命周期 (比如地址对象不存在生命周期)
    * 无状态对象
* Service /服务
    * 无状态对象
    * 当一个属性或行为放在Entity、Value Object中模棱两可或不合适的时候就需要以Service的形式来呈现


### 生命周期

* Factory （工厂）
    
    用来创建Model，以及帮助Repository (数据源)注入到Model中

* Aggreagte （聚合根）
    
    封装Model，一个Mode中l可能包含其他Model（类似一个对象中包含其他对象的引用，实际概念更为复杂些）

* Repository (数据源)

    数据源的访问网关层、通过Repository来对接不同的数据源

### 领域驱动架构

1. 三层架构、

    * API层

        作为对外打包、前端接口调用使用
    
    * Domain层

        系统的核心层，所有具体的业务逻辑处理、事件处理等都在这层域模型中处理

    * Repository 层

        数据源代理层，Repository 层类似一个网关代理，它本身没有数据，数据都是通过它的代理来被Domain层访问

    ![domain三层架构](https://pic2.zhimg.com/80/v2-3904b52986b120ea91a499b55ecf1619_720w.webp)

2. REST架构

3. 事件驱动架构

4. CQRS架构

5. 六边形架构


### 领域对象

* VO/View Object

    视图对象，主要对应界面显示的数据对象。对于一个WEB页面，或者SWT、SWING的一个界面，用一个VO对象对应整个界面的值。

* DTO/Data Transfer Object

    数据传输对象，主要用于远程调用等需要大量传输对象的地方。比如我们一张表有100个字段，那么对应的PO就有100个属性。但是我们界面上只要显示10个字段，客户端用WEB service来获取数据，没有必要把整个PO对象传递到客户端，这时我们就可以用只有这10个属性的DTO来传递结果到客户端，这样也不会暴露服务端表结构.到达客户端以后，如果用这个对象来对应界面显示，那此时它的身份就转为VO。在这里，我泛指用于展示层与服务层之间的数据传输对象。

* DO/Domain Object

    领域对象，就是从现实世界中抽象出来的有形或无形的业务实体。

* PO/Persistent Object

    持久化对象，它跟持久层（通常是关系型数据库）的数据结构形成一一对应的映射关系，如果持久层是关系型数据库，那么，数据表中的每个字段（或若干个）就对应PO的一个（或若干个）属性。最形象的理解就是一个PO就是数据库中的一条记录，好处是可以把一条记录作为一个对象处理，可以方便的转为其它对象。


<!-- Todo -->
<!-- https://mp.weixin.qq.com/s/x4HjK8t6mPAg1vQWa3PrSg -->