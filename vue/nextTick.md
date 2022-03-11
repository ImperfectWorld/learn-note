官网解释：
> 在下次DOM更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的DOM。

原理
> Vue 异步执行 DOM 更新。只要观察到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作上非常重要。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。Vue 在内部尝试对异步队列使用原生的Promise.then和MessageChannel，如果执行环境不支持，会采用setTimeout(fn, 0)代替。

实现
> next-tick.js 对外暴露了nextTick这一个参数，所以每次调用Vue.nextTick时会执行：
- 把传入的回调函数cb压入callbacks数组
- 执行timerFunc函数，延迟调用 flushCallbacks 函数
- 遍历执行 callbacks 数组中的所有函数
```bash
这里的 callbacks 没有直接在 nextTick 中执行回调函数的原因是
保证在同一个 tick 内多次执行nextTick，不会开启多个异步任务，而是把这些异步任务都压成一个同步任务，在下一个 tick 执行完毕。
```
