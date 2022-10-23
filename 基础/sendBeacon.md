# 什么是sendBeacon
[MDN解释](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator/sendBeacon)
```bash
这个方法主要用于满足统计和诊断代码的需要，这些代码通常尝试在卸载（unload）文档之前向 Web 服务器发送数据。
过早的发送数据可能导致错过收集数据的机会。然而，对于开发者来说保证在文档卸载期间发送数据一直是一个困难。
因为用户代理通常会忽略在 unload (en-US) 事件处理器中产生的异步 XMLHttpRequest。
```
使用 sendBeacon() 方法会使用户代理在有机会时异步地向服务器发送数据，同时不会延迟页面的卸载或影响下一导航的载入性能，这意味着：

- 数据发送是可靠的。
- 数据异步传输。
- 不影响下一导航的载入。

数据是通过 HTTP POST 请求发送的，支持跨域。

# 用法
```
navigator.sendBeacon(url, data);
```
上报case
```
const sendData = () => {
    const params = Object.assign({ text: '测试1' }, { time: new Date().getTime() });
    let headers = {
        type: 'application/x-www-form-urlencoded',
    };
    let blob = new Blob([JSON.stringify(params)], headers);
    navigator.sendBeacon('http://localhost:3000/log', blob);
};
window.addEventListener('visibilitychange', sendData, false);
```

# 重点关注
### 监控事件visibilitychange
网站通常希望在用户完成页面浏览后向服务器发送分析或诊断数据，最可靠的方法是在 `visibilitychange` 事件发生时发送数据.

在许多情况下（尤其是移动设备）浏览器不会产生 unload、beforeunload 或 pagehide 事件.比如以下场景：
- 用户加载了网页并与其交互。
- 完成浏览后，用户切换到了其它应用程序，而不是关闭选项卡。
- 随后，用户通过手机的应用管理器关闭了浏览器应用。

### 兼容性
IE不支持
![image](https://user-images.githubusercontent.com/11763399/197396635-64fdd57d-4599-4da1-8d7a-55508f7760e0.png)

### navigator.sendBeacon的底层实现
sendBeacon的底层是通过`fetch`api实现的。
在执行sendBeacon时，会按照以下3个步骤执行：
1、检测数据量大小。
如果sendBeacon要发送的数据量大小超过最大限制则返回false，否则进行下一步。
数据量大小限制：Chrome40-86约65536个字符，数据大小的限制64kb，这些是浏览器层面的限制，不同的浏览器可能还存在细微的差别。
2、检测mimeType类型
如果mimeType值不是CORS安全列出的请求标头值且是跨域的，则直接返回false，否则进行下一步。
3、发起fetch请求
它构造的fetch请求，参数如下：
```
fetch('/xxx', 
{ method: 'POST', 
  url:'', 
  keep-alive: true, 
  body:transmittedData, 
  mode:corsMode, 
  credentials: 'include' 
 });
```
可以注意到，Fetch API 支持一个 keep-alive 选项，
当设置为 true 时，保证不管发送请求的页面关闭与否，请求都会持续到结束。
credentials设置为include，即使是跨域调用，也总是携带cookie。

