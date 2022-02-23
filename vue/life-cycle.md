> 单一组件
```
beforeCreate () {},
created() {},
beforeMount () {},
mounted () {},
beforeUpdate () {},
updated() {},
beforeDestroy() {},
destroyed() {}
```

> 嵌套组件

### 初始化
<img width="169" alt="image" src="https://user-images.githubusercontent.com/11763399/155052658-c7298ddc-c51f-41d4-8d33-cef83c3500f2.png">

### 更新(子组件更新全局变量)
<img width="169" alt="image" src="https://user-images.githubusercontent.com/11763399/155052800-30bcf522-07c9-401f-b287-f8633b2fc4a9.png">

### 更新（只更新子组件数据，只会触发子组件生命周期）


> this.$el 和 this.$data 挂载的时机
<img width="476" alt="image" src="https://user-images.githubusercontent.com/11763399/155053982-714ac6c0-7281-4939-930a-e97c9fe3627c.png">



### 实现原理
```
// 使用
new Vue({
  el: '#app',
  data: {
    str: '你好'
  }
})

class Vue{
  constructor(options) {
    
    if (typeof options.beforeCreate === 'function') {
      options.beforeCreate.bind(this)();
    }
    this.$data = options.data;
    if (typeof options.created === 'function') {
      options.created.bind(this)();
    }
    if (typeof options.beforeMount === 'function') {
      options.beforeMount.bind(this)();
    }
    this.$el = document.querySelecter(options.el);
    
    if (typeof options.mounted === 'function') {
      options.mounted.bind(this)();
    }
  }
  
}

```
