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
