[在线code平台](https://codesandbox.io/)

## 虚拟节点 vdom
虚拟DOM就是一个用来描述真是DOM的普通的js对象
### 好处
- 组件的渲染逻辑完全从真实的DOM中解耦
  - 自定义渲染解决方案(渲染器 API)。IOS/安卓/webgl
- 以编程的方式构造/检查/克隆以及操作所需的DOM结构，在实际返回渲染引擎之前，利用js的全部能力。
  - html中模板语法限制，没有js灵活

# 待解决问题
- 如何不刷新页面，重置页面所有组件，所有组件重新请求对应接口数据？ key
  -  组件的key值改变会使vue页面重新刷新
```bash
  <template>
    <div id="app" :key="key">
      <router-view></router-view>
    </div>
  </template>
<script>
export default {
  name: 'App',
  provide () {
    return {
      reload: this.reload
    }
  },
  data () {
    return {
      key: 1
    }
  },
  methods: {
    reload () {
      this.key++
    }
  }
}
</script>
```
  - v-if状态切换

  ```js
  <template>
  <div id="app">
    <router-view v-if="isShow"></router-view>
  </div>
</template>
 
<script>
export default {
  name: 'App',
  provide () {
    return {
      reload: this.reload
    }
  },
  data () {
    return {
      isShow: true
    }
  },
  methods: {
    reload () {
      this.isShow= false
      this.$nextTick(function () {
        this.isShow= true
      })
    }
  }
}
</script>
```

- diff算法的时间复杂度，及降低时间复杂度的方法
  - 两棵树完全diff是O(n^3)， vue的diff只是同层diff O(n) [跨层级修改场景较少]
  - tagname/key
- 双向数据绑定
  - 一个组件上的 v-model 默认会利用名为 value 的 prop 和名为 input 的事件


# 组件列表中key的作用

> 官网描述：

当 Vue 正在更新使用 v-for 渲染的元素列表时，它默认使用“就地更新”的策略。
如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。

这个默认的模式是高效的，但是只适用于**不依赖子组件状态或临时 DOM 状态** (例如：表单输入值) 的列表渲染输出。

建议尽可能在使用 v-for 时提供 key attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。

### 不带key

- 判断sameVnode时因为a.key和b.key都是undefined，对于列表渲染来说已经可以判断为相同节点然后调用patchVnode了。
- 官网推荐推荐的使用key，应该理解为“使用唯一id作为key”。
因为index作为key，和不带key的效果是一样的。index作为key时，每个列表项的index在变更前后也是一样的，都是直接判断为sameVnode然后复用。
- key的作用就是**更新组件时判断两个节点是否相同**。相同就复用，不相同就删除旧的创建新的。
- 带唯一key时每次更新都不能找到可复用的节点，不但要销毁和创建vnode，在DOM里添加移除节点对性能的影响更大。

不带key时节点能够复用，省去了销毁/创建组件的开销，同时只需要修改DOM文本内容而不是移除/添加节点，这就是文档中所说的“刻意依赖默认行为以获取性能上的提升”。

#### 为什么官网要求带key？
因为这种模式只适用于渲染简单的无状态组件。对于大多数场景来说，列表组件都有自己的状态。

### 带key
- **维持组件的状态，保证组件的复用**因为有 key 唯一标识了组件，不会在每次比较新旧两个节点是否是同一个节点的时候直接判断为同一个节点，而是会继续在接下来的节点中找到 key 相同的节点去比较，能找到相同的 key 的话就复用节点，不能找到的话就增加或者删除节点。
- **查找性能上的提升**。有 key 的时候，会生成 hash，这样在查找的时候就是 hash 查找了，基本上就是 O(1) 的复杂度。
- **节点复用带来的性能提升**。因为有 key 唯一标识了组件，所以会尽可能多的对组件进行复用（尽管组件顺序不同），那么创建和删除节点数量就会变少，这方面的消耗就会下降，带来性能的提升。
