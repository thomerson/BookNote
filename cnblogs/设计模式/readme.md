## 设计模式

GOF/Gang of Four/四位作者

设计模式主要是基于以下的面向对象设计原则。

* 对接口编程而不是对实现编程。
* 优先使用对象组合而不是继承。

## 分类

* 创建型模式```Creational Patterns```
    > 在创建对象的同时隐藏创建逻辑的方式，而不是使用 new 运算符直接实例化对象。

    * 工厂方法模式```Factory Pattern```
    * 抽象工厂模式
    * [单例模式```Singleton Pattern```](https://www.cnblogs.com/thomerson/p/16795927.html)
    * 建造者模式
    * 原型模式

* 结构型模式```Structural Patterns```
    > 关注类和对象的组合
    
    * 适配器模式
    * 装饰器模式
    * 代理模式
    * 外观模式
    * 桥接模式
    * 组合模式
    * 享元模式
    
* 行为型模式
    > 关注对象之间的通信

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
