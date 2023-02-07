## class

JS 中并没有一个真正的 class 原始类型， class 仅仅只是对原型对象运用**语法糖**

class类本质上就是一个函数，自身指向的就是构造函数

每个类中包含了一个特殊的方法 constructor()，构造函数


```javascript

class Man {
  constructor(name) {
    this.name = '小豪';
  }
  console() {
    console.log(this.name);
  }
}
const man = new Man('小豪');
man.console(); // 小豪

```

## class继承

 ```extends```

  ```super() ```方法用于调用父类的构造函数

```javascript
// 基类
class Animal {
    // eat() 函数
    // sleep() 函数
};
 
 
//派生类
class Dog extends Animal {
    // bark() 函数
};
```

## getter和setter

类中可以使用 getter 和 setter 来获取和设置值，getter 和 setter 都需要在严格模式下执行

```javascript
class Runoob {
  constructor(name) {
    this.sitename = name;
  }
  get s_name() {
    return this.sitename;
  }
  set s_name(x) {
    this.sitename = x;
  }
}
```