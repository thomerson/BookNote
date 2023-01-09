## this指向


```javascript
var name="lucy";
const obj={
    name:"martin",
    say:function () {
        console.log(this.name);
    }
};
obj.say(); //martin，this指向obj对象
setTimeout(obj.say,0); //lucy，this指向window对象
```

## 改变this指向的3种方法

* apply

	```javascript
	var name="martin";
	var obj={
	name:"lucy",
	say:function(year){
	console.log(this.name+" is "+year);
	}
	};
	var say=obj.say;
	setTimeout(function(){
	say.apply(obj,["18"])
	} ,0); //lucy is 18,this改变指向了obj
	say("20") //martin is 20,this指向window，说明apply只是临时改变一次this指向
	```
* call

	```javascript
	var name="martin";
	var obj={
	name:"lucy",
	say:function(year,place){
		console.log(this.name+" is "+year+' from '+place);
	}
	};
	var say=obj.say;
	setTimeout(function(){
	say.call(obj,18,'china')
	} ,0); // lucy is 18 from china,this改变指向了obj
	say(20,"火星") // martin is 20 from 火星,this指向window，说明call只是临时改变一次this指向
	```

    call和apply的区别是**call以参数列表的形式传入，而apply以参数数组的形式传入。**


* bind

	```javascript
	var name="martin";
	var obj={
	name:"lucy",
	say:function(year,place){
	console.log(this.name+" is "+year+' from '+place);
	}
	};
	var say=obj.say.bind(obj,18)
	this.say();//lucy is 18 from undefined
	this.say("火星");//lucy is 18 from 火星
    ```

    bind方法和call很相似，后面传入的也是一个参数列表(但是这个参数列表可以分多次传入，call则必须一次性传入所有参数)

    但是它改变this指向后**不会立即执行**，而是**返回一个永久改变this指向的函数**



