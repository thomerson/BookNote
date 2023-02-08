## ES Modules

**模块化**

用于处理模块的 ECMAScript 标准。

采用ES Module将自动采用*严格模式*

```javascript
// 模块 A 导出一个方法
export const sub = (a, b) => a + b;
// 模块 B 导入使用
import { sub } from './A';
console.log(sub(1, 2)); // 3
```

### export导出的几种方式

* 变量在声明时直接导出，即export后跟变量声明语句

```javascript
// export
export const name = "why";
export const age = 18;

export function sum(a, b) {
  return a + b;
}

export class Person {
  constructor(name) {
    this.name = name;
  }
}

// import
import { name, age, sum, Person } from "./foo.js";
console.log(name, age);

```

* 变量声明后，统一导出

```javascript

// export
const name = "why";
const age = 18;

function sum(a, b) {
  return a + b;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

//export关键字后面的{}不是一个对象，而是一种语法
export { name, age, sum, Person };

//import
import { name, age, sum, Person } from "./foo.js";
console.log(name, age);

```

3. 统一导出时给导出的变量起别名

```javascript
const name = "why";
const age = 18;

function sum(a, b) {
  return a + b;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

//3.统一导出时使用as关键字给变量起别名
export { name as bName, age as bAge, sum as bSum, Person as BPerson };
```

4. export default 默认导出

    * 默认导出export时可以不需要指定名字；
    * 在导入时不需要使用 {}，并且可以自己来指定名字；
    * 默认导出只能有一个

```javascript
const height = 1.88;

// 写法1
export default height;

// 写法2
export {
  height as default
};


// 导入模块的默认导出时，直接导入即可
import height from "./demo.js";

```


### import 使用注意

* 不可以放到逻辑代码中

```javascript
 // 错误写法
if(true){
    import height from "./demo.js";
}
// 正确写法，使用import函数代替
if(true){
    import('./demo.js').then(res=>{
        console.log(res);
    });
}

```