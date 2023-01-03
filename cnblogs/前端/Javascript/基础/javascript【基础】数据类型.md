## 五种基本数据类型

* Number

* String

* Boolean

* Undefined

    一个没有设置值的变量

* Null 

    表示一个空对象引用

ES6(Symbol)  //ES6

## 一种复杂数据类型

* Object

> 在 Javascript 的逻辑运算中，0、""、null、false、undefined、NaN 都会判定为 false ，而其他都为 true 

### 两种方式访问对象属性:

```javascript
person.lastName;

person["lastName"];

```

通过for in遍历对象属性

```javascript
var person={fname:"John",lname:"Doe",age:25}; 
for (x in person)
{
    txt=txt + person[x];
}
```



## typeof

使用 typeof 操作符来检测变量的数据类型。

返回结果为js基本的数据类型

```javascript

var temp=null; typeof temp;           // 返回 object
typeof "John"                // 返回 string 
typeof 3.14                  // 返回 number
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
typeof NaN                    // 返回 number
typeof new Date()             // 返回 object
typeof function () {}         // 返回 function
typeof null                   // 返回 object
typeof myCar                  // 返回 undefined (if myCar is not declared)
//使用typeof的一个不好的地方就是它会把Array还有用户自定义函数都返回为object

```

### NaN

```javascript
typeof NaN //number
```

请使用 ```isNaN()``` 来判断一个值是否是数字。原因是 NaN 与所有值都不相等，包括它自己


## Undefined 和 Null


* null表示“没有对象”，即**此处不应该有值**。

    1. 作为函数的参数，表示该函数的参数不是对象

    2. 作为对象原型链的终点。

     ```Object.getPrototypeOf(Object.prototype)   // null```

* undefined表示“缺少值”，即**此处应该有一个值，但是还没有定义** 。

    1. 变量被声明了，但没有赋值时，就等于undefined。

    ```javascript
      var a;
      a  //    undefined
    ```

    2. 调用函数时，应该提供的参数没有提供，该参数等于undefined。
    ```javascript
     function f(x){console.log(x)}
     f()  //  undefined
     ```

    3. 对象没有赋值的属性，该属性的值为undefined。
    ```javascript
     var o = {};
     o.p // undefined
   ```

    4. 函数没有返回值时，默认返回undefined。
    ```javascript
     function f() {console.log(1)} 
     var a = f();
      a   //   undefined
   ```

```javascript
var str;
alert(str);//undefined  没有申明对象类型

//Undefined 和 Null 的区别
typeof undefined             // undefined
typeof null                  // object
null === undefined           // false
null == undefined            // true
 

```

## constructor 属性

constructor 属性返回所有 JavaScript 变量的构造函数。

```javascript
"John".constructor                 // 返回函数 String()  { [native code] }

(3.14).constructor                 // 返回函数 Number()  { [native code] }

false.constructor                  // 返回函数 Boolean() { [native code] }

[1,2,3,4].constructor              // 返回函数 Array()   { [native code] }

{name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }

new Date().constructor             // 返回函数 Date()    { [native code] }

function () {}.constructor         // 返回函数 Function(){ [native code] }


```

constructor只能对已有变量进行判断，而typeof则可对未声明变量进行判断（返回undefined）。

 

 

## instanceof

为判断一个对象是否为某一数据类型，或一个变量是否为一个对象的实例;返回boolean类型

语法为 ```o instanceof A```

```javascript
var h=new Person();
var o={};

alert("h instanceof Person:" + (h instanceof Person));//true

alert("h instanceof Object:" + (h instanceof Object));//true

alert("o instanceof Object:" + (o instanceof Object));//true

```

