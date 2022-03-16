
```html
<div id="app"></div>

<script>
function h (tag, props, children) {
    return {
        tag,
        props,
        children
    }
}

// 挂载
function mount (vnode, container) {
    const el = vnode.el = document.createElement(vnode.tag);
    // props
    if (vnode.props) {
        for (const key in vnode.props) {
            const value = vnode.props[key];
            if (key.startsWith('on')) {
                el.addEventListener(key.slice(2).toLowerCase(), value)
            }
            else {
                el.setAttribute(key, value);
            }
        }
    }
    // children
    if (vnode.children) {
        if (typeof vnode.children === 'string') {
            el.textContent = vnode.children;
        }
        else {
            vnode.children.forEach(child => {
                mount(child, el);
            });
        }
    }
    container.appendChild(el);
}

function path(n1, n2) {
    // 通过编译器得到该元素只包含静态的props，跳过检查
    const el = n2.el = n1.el;
    // tag是否相同
    if (n1.tag === n2.tag) {
        const oldProps = n1.props || {}
        const newProps = n2.props || {}
        for (const key in newProps) {
            const oldValue = n1.props[key]
            const newValue = n2.props[key]
            if (newValue !== oldValue) {
                el.setAttribute(key, newValue);
            }
        }
        for (const key in oldProps) {
            if (!(key in newProps)) {
                el.removeAttribute(key)
            }
        }

        // children
        const oldChildren = n1.children;
        const newChildren = n2.children;

        if (typeof newChildren === 'string') {
            if (typeof oldChildren === 'string') {
                if (newChildren !== oldChildren) {
                    el.textContent = newChildren
                }
            }
            else {
                el.textContent = newChildren
            }
        }
        else {
            if (typeof oldChildren === 'string') {
                el.innerHTML = ''
                newChildren.forEach(child => {
                    mount(child, el)
                })
            }
            else {
                const commonLength = Math.min(oldChildren.length, newChildren.length)
                for (let i = 0; i < commonLength; i++) {
                    path(oldChildren[i], newChildren[i]);
                }
                if (newChildren.length > oldChildren.length) {
                    newChildren.slice(oldChildren.length).forEach(child => {
                        mount(child, el)
                    })
                }
                else if (newChildren.length < oldChildren.length) {
                    oldChildren.slice(newChildren.length).forEach(chlid => {
                        el.removeChild(child)
                    })
                }
            }
        }
    }
    else {

    }
}

// reactivity

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
    // vue3
    return new Proxy(raw, reactiveHandlers)
}

function watchEffect(effect) {
    activeEffect = effect
    effect()
    activeEffect = null
}

const app = {
    data: reactive({
        count: 0
    }),
    render() {
        return h('div', {
            onClick: () => this.data.count++,
        }, String(this.data.count))
    }
}

function mountApp(component, container) {
    let isMounted = false
    let prevVdom
    watchEffect(() => {
        if (!isMounted) {
            prevVdom = component.render()
            mount(prevVdom, container)
            isMounted = true
        }
        else {
            const newVdom = component.render()
            path(prevVdom, newVdom)
            prevVdom = newVdom
        }
    })
}

mountApp(app, document.getElementById('app'))
</script>
```
