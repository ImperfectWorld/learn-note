# Vuex

### 原理实现
```JavaScript
// 1.插件 挂载$store
// 2.实现Store

let Vue;

class Store {
    constructor(options) {
        // state响应式处理
        // this.$store.state
        this._vm = new Vue({
            data: {
                $$state: options.state
            }
        });

        this._mutations = options.mutations;
        this._actions = options.actions;

        this.commit = this.commit.bind(this);
        this.dispatch = this.dispatch.bind(this);

        this.getters = {};

        options.getters && this.handleGetters(options.getters);
    }

    handleGetters(getters) {
        Object.keys(getters).map(key => {
            Object.defineProperty(this.getters, key, {
                get: () => getters[key](this.state)
            })
        })
    }

    get state() {
        return this._vm._data.$$state;
    }

    set state(v) {
        console.error();
    }

    commit(type, payload) {
        const entry = this._mutations[type];

        if (!entry) {
            console.error();
        }

        entry(this.state, payload);
    }

    dispatch(type, payload) {
        const entry = this._actions[type];

        if (!entry) {
            console.error();
        }

        entry(this, payload);
    }
}

const install = (_Vue) => {
    Vue = _Vue;

    // 全局混入
    Vue.mixin({
        beforeCreate() {
            if (this.$options.store) {
                Vue.prototype.$store = this.$options.store;
            }
        }
    });
};

export default { Store, install };
```

### 使用
```JavaScript
import Vuex from './vuex'
// this.$store
Vue.use(Vuex)

export default new Vuex.Store({
    state: {
        num: 0
    },
    mutations: {
        // state哪里来的
        add(state) {
            return state.num++;
        }
    },
    actions: {
        // 参数是什么，哪里来的
        add({commit}) {
            setTimeout(() => {
                commit('add');
            }, 1000)
        }
    },
    getters: {
        doubleCounter(state) {
            return state.num * 2;
        }
    }
})

```
