## 计算computed

有一些数据需要随着其它数据变动而变动时


## 侦听Watch

当需要在数据变化时执行**异步**或**开销较大**的操作时,用```watch```

### watch 深度监控

```javascript
watch: {
    'cityName.name': {
      handler(newName, oldName) {
      // ...
      },
      deep: true,
      immediate: true
    }
  }
```