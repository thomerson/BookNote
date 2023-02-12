## Vue-router

1. 安装

```shell
npm install vue-router --save-dev 
```

vue-cli中已经选择安装了vue-router，那这里不需要重复安装了

2. 解读route

路径```src/router/index.js``

```javascript

import Vue from 'vue'   //引入Vue
import Router from 'vue-router'  //引入vue-router
import HelloWorld from '@/components/HelloWorld'  //引入根目录下的HelloWorld.vue组件

Vue.use(Router)  //Vue全局使用Router

export default new Router({
  routes: [                   //配置路由，这里是个数组
    {                         //每一个链接都是一个对象
      path: '/',              //链接路径
      name: 'HelloWorld',     //路由名称，
      component: HelloWorld   //对应的组件模板
    }
  ]
})

```


```html
<router-link to="/">首页</router-link>


```

3. 配置子路由

```javascript

import Vue from 'vue' //引入Vue
import Router from 'vue-router' //引入vue-router
import HelloWorld from '@/components/HelloWorld' //引入根目录下的HelloWorld.vue组件
import Hi from '@/components/Hi'
import Hi1 from '@/components/Hi1'
import Hi2 from '@/components/Hi2'


Vue.use(Router) //Vue全局使用Router

export default new Router({
  routes: [ //配置路由，这里是个数组
    { //每一个链接都是一个对象
      path: '/', //链接路径
      name: 'HelloWorld', //路由名称，
      component: HelloWorld //对应的组件模板
    },
    {
      path: '/hi',
      name: 'Hi',
      component: Hi,
      children: [
        { path: '/', name: 'hi', component: Hi }, 
        { path: 'hi1', name: 'hi1', component: Hi1 }, 
        { path: 'hi2', name: 'hi2', component: Hi2 }
      ]
    }
  ]
})
```

4. 参数传递

* 用name传递参数

```javascript
routes: [ //配置路由，这里是个数组
  { //每一个链接都是一个对象
    path: '/', //链接路径
    name: 'HelloWorld', //路由名称，
    component: HelloWorld //对应的组件模板
  }]
```

```html
<p>{{$route.name}}</p>
```

* 通过 标签中的to传参

```html
<router-link :to="{name:xxx,params:{key:value}}">valueString</router-link>
```


```html
<p>{{$route.params.username}}</p>
```

* 利用url传递参数

    * ```:```冒号的形式传递参数
		```javascript
		{
		path: '/params/:newsId/:newsTitle',
		name: 'Params',
		component:Params
		}
		```
		
		```html
		<!-- 传参 -->
		<router-link to="/params/aaa/bbb">Params</router-link>
		
		<!-- 接收参数 -->
		<div>
			<p>新闻ID:{{$route.params.newsId}}</p>
			<p>新闻标题：{{$route.params.newsTitle}}</p>
		</div>
		```


    * 正则表达式在URL传值中的应用

    ```javascript
    path:'/params/:newsId(\\d+)/:newsTitle', //newsId 限制数字
    ```


5. ```redirect```重定向

6. ```alias```别名

* redirect：直接改变了url的值，把url变成了真实的path路径。

* alias：URL路径没有别改变，这种情况更友好，让用户知道自己访问的路径，只是改变了<router-view>中的内容

7. ```transition```过渡动画

```html
<transition name="fade">
  <router-view/>
</transition>
```

组件过渡过程中，会有四个CSS类名进行切换，这四个类名与transition的name属性有关，比如name=”fade”,会有如下四个CSS类名：

* fade-enter

    进入过渡的开始状态，元素被插入时生效，只应用一帧后立刻删除。
* fade-enter-active
    进入过渡的结束状态，元素被插入时就生效，在过渡过程完成后移除。
* fade-leave
    离开过渡的开始状态，元素被删除时触发，只应用一帧后立刻删除。
* fade-leave-active
    离开过渡的结束状态，元素被删除时生效，离开过渡完成后被删除。

```css
.fade-enter {
  opacity:0;
}
.fade-leave{
  opacity:1;
}
.fade-enter-active{
  transition:opacity .5s;
}
.fade-leave-active{
  opacity:0;
  transition:opacity .5s;
}
```

8. ```mode```模式

* ```histroy```

    url没有```#```号，浏览历史记录栈的API实现

* ```hash``` 

    **默认**，监听location对象hash值变化事件来实现```window.onhashchange```

9. 路由钩子函数

    * ```beforeEnter``` 写在路由文件中，在进入此路由配置时

		```javascript
		{
			path: '/params/:newsId(\\d+)/:newsTitle',
			name: 'Params',
			component: Params,
			beforeEnter:(to,from,next)=>{
				console.log('我进入了params模板');
				console.log(to);
				console.log(from);
				next();
			}
		}
		
		```

    * ```beforeRouteEnter``` 写在模板中，在路由进入前

    * ```beforeRouteLeave``` 写在模板中，在路由离开前

		```javascript
	
		export default {
			data() {
				return {
				msg: "params pages"
				};
			},
			beforeRouteEnter(to, from, next) {
				console.log("准备进入路由模板");
				next();
			},
			beforeRouteLeave(to, from, next) {
				console.log("准备离开路由模板");
				next();
			}
		};
		```

10. 编程式导航

```javascript
router.go(-1) //代表着后退，我们可以让我们的导航进行后退，并且我们的地址栏也是有所变化的。

router.go(1) //前进

this.$router.push('/xxx')//跳转
```

