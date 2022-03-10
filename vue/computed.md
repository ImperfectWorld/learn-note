## 分析
计算属性属于vue响应式实现的一种，本质还是一个watcher。与data不同的是监听多个属性。

核心点：

- 只有取值的时候才会执行computed的逻辑 
  - 通过一个标记this.lazy 实现，如果只是修改了依赖项，没有取值则lazy = true
- 缓存
  - 通过一个标记this.dirty, dirty为true的时候，代表是脏数据，需要执行computed的逻辑，为false的时候则表示依赖项没有更新，直接取缓存。
  - this.lazy = true 的时候，this.dity = true;

## vue computed实现

初始化 initComputed
```javascript
export function initState (vm: Component) {
  vm._watchers = []
  const opts = vm.$options
  if (opts.props) initProps(vm, opts.props)
  if (opts.methods) initMethods(vm, opts.methods)
  if (opts.data) {
    initData(vm)
  } else {
    observe(vm._data = {}, true /* asRootData */)
  }
  if (opts.computed) initComputed(vm, opts.computed)
  if (opts.watch && opts.watch !== nativeWatch) {
    initWatch(vm, opts.watch)
  }
}
```

```
function createComputedGetter (key) {
  return function computedGetter () {
    const watcher = this._computedWatchers && this._computedWatchers[key]
    if (watcher) {
      if (watcher.dirty) {
        watcher.evaluate()
      }
      if (Dep.target) {
        watcher.depend()
      }
      return watcher.value
    }
  }
}
```

