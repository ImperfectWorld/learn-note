## compiler 编译
[Vue 3 Template Explorer](https://vue-next-template-explorer.netlify.app/#eyJzcmMiOiI8ZGl2PkhlbGxvIFdvcmxkPC9kaXY+Iiwib3B0aW9ucyI6e319)

### vue3优势
- diff算法的优化： 增加静态标记，只比较动态数据所在的节点
- hoistStatic 静态提升： vue3对于不参与更新的元素，会做静态提升，只会被创建一次，在渲染时直接复用即可。
- ssr渲染性能更好： 用buffer而不是虚拟dom
- 更好的Ts支持
- 按需编译，体积比vue2.x更小
- 支持多根节点组件
