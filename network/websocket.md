# websocket
WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

在 WebSocket API 中，浏览器和服务器只需要做一个握手的动作，然后，浏览器和服务器之间就形成了一条快速通道。两者之间就直接可以数据互相传送。

[MDN接口介绍](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)

```javascript
// node层服务
const ws = require('nodejs-websocket');

const server = ws
    .createServer(function (socket) {
        let count = 1;
        socket.on('text', function (str) {
            console.log(str);
            socket.sendText('服务器收到客户端发来的消息' + count++);
        });
    })
    .listen(3000);
```

```html
<script>
    let ws = new WebSocket('ws://localhost:3000/');

    ws.onopen = function () {
        setInterval(() => {
            ws.send('客户端消息');
        }, 2000);
    };

    ws.onmessage = function (e) {
        console.log(e.data);
    };
</script>
```
