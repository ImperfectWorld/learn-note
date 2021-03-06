
```html
<!-- 挂载 -->
<style>
    .red {
        color: red;
    }
</style>

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
                el.setAttribute(key, value);
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

    const vdom = h('div', { class: 'red'}, [
        h('span', null, 'hello')
    ])

    mount(vdom, document.getElementById('app'))
</script>
```
