## UnionAPP

一个使用**Vue.js**开发所有前端应用的框架

可发布到iOS、Android、Web（响应式）、以及各种**小程序**（微信/支付宝/百度/头条/飞书/QQ/快手/钉钉/淘宝）、快应用等多个平台


### 类似产品

* [taro](https://taro-ui.jd.com/#/)

    JD旗下

* [flutter](https://flutter.cn/)

    使用了自带绘制引擎的架构设计。

    带来的好处就是原生基本的性能和极高的不同端一致性效果

    r可以保证用户体验和原生页面一致，不会有H5或者uni-app造成的用户体验明显下降的问题可以保证用户体验和原生页面一致，不会有H5或者uni-app造成的用户体验明显下降的问题

### 开发工具

```HBuilderX```

## 应用生命周期

* onLaunch 当uni-app **初始化完成时触发**（全局只触发一次)

* onShow 当 uni-app **启动**，或从后台进入前台显示

* onHide 当 uni-app 从前台进入后台

## 页面生命周期

* onLoad	监听页面加载，其参数为上个页面传递的数据，参数类型为Object（用于页面传参

* onShow	监听页面显示

* onReady	监听页面初次渲染完成

* onHide	监听页面隐藏

* onUnload	监听页面卸载

* onPullDownRefresh	监听用户下拉动作 ，一般用于下拉刷新

* onReachBottom	页面上拉触底事件的处理函数

* onPageScroll	监听页面滚动 ，参数为 Object

* onTabItemTap	当前是 tab 页时，点击 tab 时触发。

* onShareAppMessage	用户点击右上角分享	微信小程序



## 页面适配

通用 css 单位包括 

* ```px```

* ```rpx```响应式 px


## 路由与页面跳转

* ```uni.navigateTo``` 

    保留当前页面，跳转到应用内的某个页面，使用uni.navigateBack可以返回到原页面

* ```uni.redirectTo```

    关闭当前页面，跳转到应用内的某个页面

* ```uni.reLaunch```

    关闭所有页面，打开到应用内的某个页面


* ```uni.preloadPage```

    预加载页面，是一种性能优化技术。被预载的页面，在打开时速度更快
    

## 网络请求

```uni.request```

