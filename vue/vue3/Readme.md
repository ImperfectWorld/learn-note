# 跟着作者学vue3系列
[跟尤雨溪一起解读Vue3源码](https://www.bilibili.com/video/BV1rC4y187Vw?spm_id_from=333.337.search-card.all.click)

[jsonplaceholder](https://jsonplaceholder.typicode.com/)

## vue3优势
- diff算法的优化： 增加静态标记，只比较动态数据所在的节点
- hoistStatic 静态提升： vue3对于不参与更新的元素，会做静态提升，只会被创建一次，在渲染时直接复用即可。
- ssr渲染性能更好： 用buffer而不是虚拟dom
- 更好的Ts支持
- 按需编译，体积比vue2.x更小
- 支持多根节点组件

## vue3 编译器优化
- 区分动态节点&静态节点，给动态节点打上补丁标志
- 静态提升：将静态节点提升到渲染函数以外，减少更新时创建虚拟DOM带来的性能开销和内存占用
- 预字符串化：将静态节点进行字符串化，减少创建虚拟节点产生的性能开销以及内存占用
- 缓存内联事件处理函数：避免造成不必要的组件更新
- v-once指令：缓存全部或者部分虚拟节点，避免组件更新时重新创建虚拟DOM带来的性能开销，也可以避免无用的diff操作。
