## class

JS 中并没有一个真正的 class 原始类型， class 仅仅只是对原型对象运用**语法糖**

class类本质上就是一个函数，自身指向的就是构造函数


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