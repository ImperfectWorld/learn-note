
> 找出最小数量的需要执行的dom操作

```javascript
 function path(n1, n2) {
    console.log('path');
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

const vdom = h('div', { class: 'red'}, [
    h('span', null, 'hello')
])

const vdom2 = h('div', { class: 'green'}, [
    h('span', null, 'changed!')
])

path(vdom, vdom2)
```
