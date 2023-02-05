## 类函数 对象函数 原型函数

对象方法和原型方法属于实例方法，

区别在于对象方法在new对象时，都会在内存中产生新的副本

将有共性的方法做成原型方法，将有个性的方法做成对象方法

```javascript

// 1. 申明
function Class(){   //声明一个类

    this.constructMethod = function(){};  //添加对象方法
};

Class.classcMethod = function(){}; //添加类（静态）方法 

Class.prototype.protocMethod=function(){};//添加原型方法

// ***********************

// 2. 使用
Class.classMethod();//类方法直接调用

var instance = new Class();

instance.constructMethod();//构造方法实例才能调用

instance.protoMethod();//原型方法实例才能调用
```

3，性能上的区别：

* 类方法在内存中只会有一份，因为它只属于类本身，在实际中，我们一般不会用到类方法。写出来主要是让你知道它而已。

* 构造方法和原型方法都是实例的，但是对象方法会在每一次new Class()时，都在内存中产生一个新的副本。通常这种方法我们用在实例间的不同之处。每个实例的构造方法互不影响。但是显然，它又占据内存了。原型方法就正好相反，它不会随着new Class()时产生新的副本，它在内存中也只有一份。可以实现实例间的共享。同时也节约了内存。

综上：你在开发时，一般不会用到类方法，将有共性的方法做成原型方法，将有个性的方法做成对象方法。

4. 以上谈到的构造方法，在实际项目中用还可以将它转移到实例上，即给实例添加方法，因为通常我们只在类的构造函数里放一些属性成员，而不是方法。见下：

 
```javascript
function Class(){};

var instance = new Class();

instance.instanceFn=function(){};//添加实例方法
```
