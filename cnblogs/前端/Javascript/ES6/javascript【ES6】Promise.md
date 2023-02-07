## ```Promise```

解决**回调地狱(callback hell)**问题

使用promise封装的axios请求

```javascript
 axios
      .get(url1, {})
      .then((res1) => {
        //获取到第一个数据
        console.log(res1);
        //根据第一个数去去获取第二个数据
        return axios.get(url2, { query: res1.xxx });
      })
      .then((res2) => {
        //获取到第一个数据
        console.log(res2);
        //根据第二个数去去获取第三个数据
        return axios.get(url3, { query: res2.xxx });
      })
      .then((res3) => {
        //获取到第三个数据
        console.log(res3);
        //...
      })
      .catch((err) => {
        console.log(err);
      });

```


Promise是现代异步编程的基础,在Promise返回给我们的时候操作其实还没有完成，但Promise对象可以让我们操作最终完成时对其进行处理，无论成功还是失败。


Promise的返回三种状态

* 等待```pending```
* 成功```fulfilled```
* 拒绝```rejected```


### 基本使用

```javascript
new Promise((resolve, reject) => {
      console.log(1);
    });
    console.log(2);
```

### 链式调用


多个请求按顺序调用用then连接

```javascript

new Promise((resolve, reject) => {
  //这里一般会有一个网络请求或其它异步操作
  resolve("成功的值1");
})
  .then((res) => {
    console.log(res); //成功的值1
    return Promise.resolve("成功的值2");
  })
  .then((res) => {
    console.log(res); //成功的值2
    return Promise.resolve("成功的值3");
  })
  .then((res) => {
    console.log(res); //成功的值3
    //以此类推...
  });

```

### all函数，race函数和any函数


多个接口请求等待

* all 全部请求成功
* race 它接收的promise实例中谁快就用谁的结果，不管你的结果是resove的还是reject
* any 只要其中的一个promise实例成功，就返回那个已经成功的promise，只有所有的promise实例都失败才会返回失败的（reject）的数组


```javascript

// all 全部请求成功
// race 它接收的promise实例中谁快就用谁的结果，不管你的结果是resove的还是reject
// any 只要其中的一个promise实例成功，就返回那个已经成功的promise，只有所有的promise实例都失败才会返回失败的（reject）的数组

Promise.all([ 
  new Promise((resolve, reject) => {
    //请求A接口，这里用setTimeout模拟请求
    setTimeout(() => {
      resolve("A的结果");
    }, 2000);
  }),
  new Promise((resolve, reject) => {
    //请求B接口，这里用setTimeout模拟请求
    setTimeout(() => {
      resolve("B的结果");
    }, 1000);
  }),
])
  .then((res) => {
    console.log(res[0]); //A的结果
    console.log(res[1]); //B的结果
    //根据A和B的结果请求C接口数据
    setTimeout(() => {
      console.log("C的请求结果");
    }, 100);
  })
  .catch((err) => {
    console.log(err);
  });

```

### async和await

async和await就是promise的语法糖形式,可以让我们的异步代码包装成同步的形式理解。

```javascript
const getAxiosData = async () => {
  try {
    const res1 = await axios.get(url1, {});
    const res2 = await axios.get(url2, { query: res1.xxx });
    const res3 = await axios.get(url2, { query: res2.xxx });
    console.log(res3);
  } catch (err) {
    console.log(err);
  }
};
getAxiosData();

```
