## 响应式
> 利用反射和代理来创建响应式

```javascript
let activeEffect
// 依赖
class Dep {
    constructor(value) {
        this.subscribers = new Set() // 订阅者列表
        this._value = value
    }
    get value() {
        this.depend()
        return this._value
    }

    set value(newValue) {
        this._value = newValue
        this.notify()
    }
    
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

function watchEffect(effect) {
    activeEffect = effect
    effect()
    activeEffect = null
}

const dep = new Dep('hello')

watchEffect(() => {
    console.log(dep.value)
})
dep.value = 'changed'
```
