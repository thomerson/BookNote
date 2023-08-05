## 宏任务与微任务


- 同步任务：同步任务不需要进行等待可立即看到执行结果，比如console
- 异步任务：异步任务需要等待一定的时候才能看到结果，比如setTimeout、网络请求
    - 宏任务
    - 微任务

![宏任务与微任务](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8927d8c4-df31-4208-81cb-bf29eaacbd8e/Untitled.png)



执行顺序

1. 先执行同步代码
2. 遇到宏任务，放入队列
3. 遇到微任务，放入微任务队列
4. 执行栈为空
5. 将微任务入栈执行
6. 所有的微任务完成之后，取出宏任务队列来执行

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/91567398-f1de-42dd-b713-4f545abf2d7d/Untitled.png)

```js

//　例1
console.log(1)
setTimeout(function() {
  console.log(2)
  new Promise(function(resolve) {
    console.log(3)
    resolve()
  }).then(function() {
    console.log(4)
  })
})

new Promise(function(resolve) {
  console.log(5)
  resolve()
}).then(function() {
  console.log(6)
})
setTimeout(function() {
  console.log(7)
  new Promise(function(resolve) {
    console.log(8)
    resolve()
  }).then(function() {
    console.log(9)
  })
})
console.log(10)

//1,5,10,6,2,3,4,7,8,9

//例2
console.log(1)

  setTimeout(function() {
    console.log(2)
  }, 0)

  const p = new Promise((resolve, reject) => {
    console.log(3)
    resolve(1000) // 标记为成功
    console.log(4)
  })

  p.then(data => {
    console.log(data)
  })

  console.log(5)

//1,3,4,5,1000,2

//例3
const p = new Promise((resolve, reject) => {
	console.log(1)
	setTimeout(() => {
		console.log('timerStart')
		resolve('resolve')
		console.log('timerEnd')
	}, 0)
	console.log(2)
})

p.then((res) => {
	console.log(res)
})

console.log('end')
// 1 2 end timerStart timerEnd resolve

// 例4
console.log('start')

const promise1 = Promise.resolve().then(() => {
	console.log('promise1')
	const timer2 = setTimeout(() => {
		console.log('timer2')
	}, 0)
})

const timer1 = setTimeout(() => {
	console.log('timer1')
	const promise2 = Promise.resolve().then(() => {
		console.log('promise2')
	})
}, 0)

console.log('end')
// start end promise1 timer1 promise2 timer2

```

总结

1. JavaScript 引擎总是先执行同步代码，然后在执行异步代码（同步先行，异步靠后）
2. 微任务的优先级高于宏任务
3. 微任务可以在 Event Loop 中插队