## compiler

### 提升

```javascript
<div>
  <div>Hello World</div>
  <span></span>
  <div>{{msg}}</div>
</div>

// 编译后结果 _hoisted_1  _hoisted_2 静态不会变化，在编译阶段进行提升
// 告诉运行时，这些内容可以跳过
import { createElementVNode as _createElementVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createElementBlock as _createElementBlock } from "vue"

const _hoisted_1 = /*#__PURE__*/_createElementVNode("div", null, "Hello World", -1 /* HOISTED */)
const _hoisted_2 = /*#__PURE__*/_createElementVNode("span", null, null, -1 /* HOISTED */)

export function render(_ctx, _cache, $props, $setup, $data, $options) {
  return (_openBlock(), _createElementBlock("div", null, [
    _hoisted_1,
    _hoisted_2,
    _createElementVNode("div", null, _toDisplayString(_ctx.msg), 1 /* TEXT */)
  ]))
}

// Check the console for the AST
```

当创建一个新节点的时候，如果节点是动态的，会加一个补丁节点的标志（例如上面的/* TEXT */ ,该节点应该被追踪 ）。无论dom节点多复杂，块（_createElementBlock）只跟踪补丁节点，放置在一个扁平的数组中。被提升的部分为虚拟节点（_createElementVNode）

> diff的时候不需要遍历所有的节点，只需要寻找block。然后根据补丁节点信息知道需要变更什么，比如TEXT，只需要关心文本就好了。
