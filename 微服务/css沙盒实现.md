# css沙盒实现-shadowDom
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div>
        <p>hello world</p>
        <div id="shadow"></div>
    </div>

    <script>
        let shadowDom = shadow.attachShadow({mode: 'closed'}); //外界无法访问
        let pElm = document.createElement('p');
        pElm.innerText = 'hello lee';
        let styleElm = document.createElement('style');
    
        styleElm.textContent = `
            p{color: red}
        `;
    
        shadowDom.appendChild(styleElm);
        shadowDom.appendChild(pElm);
    </script>
```
## shadow dom 缺点

- iconffont 字体在子应用无法加载；


Fix the problem of @font-face registration failure in shadow dom mode #1061
@font-face doesn’t work with Shadow DOM?
Icon Fonts in Shadow DOM


shadow dom 不支持 @font-face ，引入 iconfont 的时候，尽管可以引入样式，但由于字体文件是不存在的，所以相对应的图标也无法展示。
解决： 把字体文件放在主应用加载/使用通用的字体文件。


- 组件库动态创建的元素无法使用自己的样式；
对话框或提示窗是通过 document.body.appendChild 添加的，所以 shadow dom 内引入的 CSS 是无法作用到外面元素的，比如说常见的弹窗组件，Popover、Popconfirm等。
解决： 代理 document.body.appendChild 方法，即把新加的元素添加到 shadow dom容器下，而不是最外面的 body节点下


   function proxy(shadowDom) {
     if (!document.body.appendChild.isProxy) {
       document.body.appendChild = new Proxy(document.body.appendChild, {
         apply(target, thisArg, node) {
           if (node[0].classList.contains('el-overlay')) {
             target.apply(thisArg, node);
           } else {
             shadowDom.appendChild(node[0]);
           }
         },
         get: isProxy,
       });
     }
   }
复制代码


- 事件代理；
列如react组件上声明的事件最终绑定到了document这个DOM节点上，事件代理监听器依靠冒泡来知道何时处理事件。由于 Shadow DOM 事件不会冒泡到顶层，所以代理监听器不会起作用。
思路： 在渲染一个独立的 react 应用程序时，可以检测元素是否在 Shadow DOM 中。然后在 shadow root 上注册顶级侦听器。

Events not registered inside shadow dom

- svg 不生效；

### experimentalStyleIsolation

各种奇怪样式问题；

[💤 子应用 var 变量丢失](#子应用 var 变量丢失)。

同shadow dom 第二条（document.body.appendChild）;
其他；

[参考](https://juejin.cn/post/7067088168553545742#heading-18)
