### 使用方式
```
<div id="app">
  {{str}}
  <h1>{{str}}</h1>
</div>

new Vue({
  el: '#app',
  data: {
    str: '你好'
  }
})
```

### 实现原理
```
class Vue{
  constructor(options) {
    this.$el = document.querySelector(options.el);
    this.$data = options.data;
    
    this.compile(this.$el);
  }
  
  compile(node) {
    node.childNodes.forEach((item, index) => {
      // 元素节点
      if(item.nodeType === 1) {
        this.compile(item);
      }
      // 文本节点，如果有{{}}就替换数据
      if (item.nodeType === 3) {
        // 正则匹配{{}}
        let reg = /\{\{.*?\}\}/g;
        let text = item.textContent;
        item.textContent; = text.replace(reg, (match, vmKey) => {
          vmKey = vmKey.trim();
          return this.$data[vmKey];
        })
      }
    })
  }
  
}

```

> 底层还是操作的dom
