# vue-router

### 重点

- Vue.util.defineReactive => 响应式遍历current
- hashchange 监听hash变化修改current
- Vue.mixin 全局混入，获取routers参数
- render(h) 虚拟节点渲染
- Vue.component(component) 全局注册组件

### 实现
```javascript
// 实现一个插件
// 实现两个全局注册的组件

let Vue; // 保存vue的构造函数，在插件中使用

class VueRouter {
    constructor(options) {
        this.$options = options;

        // 把this.current变成响应式数组
        let initial = window.location.hash.slice(1) || '/';

        Vue.util.defineReactive(this, 'current', initial);

        window.addEventListener('hashchange', () => {
            this.current = window.location.hash.slice(1) || '/';
            console.log('current:', this.current)
        }, false);
    }
}

VueRouter.install = (_Vue) => {
    Vue = _Vue;

    // 1.挂载$router属性
    // this.$router.push()
    // 全局混入： 延迟下面的逻辑到router创建完毕并且附加到选项上时在执行
    Vue.mixin({
        beforeCreate() {
            // 此钩子再每个组件创建实例的时候会被调用
            // 根实例才有改选项
            if (this.$options.router) {
                Vue.prototype.$router = this.$options.router;
            }
        }
    });

    // 实现两个组件
    // router-link => a标签
    Vue.component('router-link', {
        props: {
            to: {
                type: String,
                required: true
            }
        },
        render(h) {
            return h(
                'a', 
                {
                    attrs: {
                        href: `#${this.to}`
                    }
                },
                this.$slots.default
            )
        }
    });
    Vue.component('router-view', {
        render(h) {
            let component = null;

            const current = this.$router.current;

            const route = this.$router.$options.routes.find(
                (route) => route.path === current
            )
            console.log('route:', route);

            if (route) {
                component = route.component;
            }

            return h(component);
        }
    });
}

export default VueRouter
```

> [相关文档](https://juejin.cn/post/7126573677176422414)
