
```javascript
let activeEffect
// 依赖
class Dep {
   subscribers = new Set() // 订阅者列表
    
    depend() {
        if (activeEffect) {
            this.subscribers.add(activeEffect)
        }
    }

    notify() { // 通知订阅者
        this.subscribers.forEach(effect => {
            effect()
        })
    }
}

const targetMap = new WeakMap()

function getDep(target, key) {
    let depsMap = targetMap.get(target)
    if (!depsMap) {
        depsMap = new Map()
        targetMap.set(target, depsMap)
    }
    let dep = depsMap.get(key)
    if (!dep) {
        dep = new Dep() // 第一次访问的时候创建一个新的dep
        depsMap.set(key, dep)
    }
    return dep
}

const reactiveHandlers = {
    get(target, key, receiver) {
        const dep = getDep(target, key)
        dep.depend()
        return Reflect.get(target, key, receiver)
    },
    set(target, key, value, receiver) {
        const dep = getDep(target, key)
        const result = Reflect.set(target, key, value, receiver)
        dep.notify()
        return result
    }
}

function reactive(raw) {
    // vue2
    // Object.keys(raw).forEach(key => {
    //     const dep = new Dep()
    //     let value = raw[key]

    //     Object.defineProperty(raw, key, {
    //         get() {
    //             dep.depend()
    //             return value
    //         },
    //         set(newValue){
    //             value = newValue
    //             dep.notify()
    //         }
    //     })
    // })
    // return raw
    
    // vue3
    return new Proxy(raw, reactiveHandlers)
}

function watchEffect(effect) {
    activeEffect = effect
    effect()
    activeEffect = null
}

const state = reactive({
    count: 0,
    list: []
})
watchEffect(() => {
    console.log(state.msg)
    console.log(state.list)
}) // 0

state.count++ // 1
```
