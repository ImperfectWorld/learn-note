## 浏览器缓存
浏览器缓存按照资源请求的优先级有以下四方面
- Memory Cache
- Service Worker Cache
- HTTP Cache
- Push Cache

## http缓存 HTTP Cache
HTTP 缓存一般又分为两种，强缓存和协商缓存.

### 强缓存
判断请求是否失效主要靠两个 HTTP Header：

Expires：数据的缓存到期时间，下一次请求时，请求时间小于服务端返回的到期时间，直接使用缓存数据。

Cache-Control：可以指定一个 max-age 字段，表示缓存的内容将在一定时间后失效。

### 协商缓存

协商：与服务器协商。

第一次请求: 服务器返回数据 + 缓存标识

第n次请求: client将缓存标识发给服务器，服务器判断成功返回304，client使用本地缓存。

#### http header：
- Last-Modified：一个 Response Header，服务器在响应请求时，告诉浏览器资源的最后修改时间。
- if-Modified-Since：一个 Request Header，再次请求服务器时，通过此字段通知服务器上次请求时，服务器返回的资源最后修改时间。

服务器会通过收到的 If-Modified-Since 和资源的最后修改时间进行比对，判断是否使用缓存。

- Etag：一个 Response Header，服务器返回的资源的唯一标示
- If-None-Match：一个 Request Header，再次请求服务器时，通过此字段通知服务器客户段缓存数据的唯一标识。

服务器会通过收到的 If-None-Match 和资源的唯一标识进行对比，判断是否使用缓存。

> 资源缓存份多级：
浏览器缓存（单个用户）源服务器缓存/CDN（多个用户）

<img width="498" alt="image" src="https://user-images.githubusercontent.com/11763399/155143209-ad360870-5a4a-491d-b262-79dd7ef77732.png">
【图片来源：code秘密花园】

### ETag/If-Not-Match是在HTTP/1.1出现的，主要是解决以下问题：

- Last-Modified标注的最后修改只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，它将不能准确标注文件的修改时间

- 如果某些文件被修改了，但是内容并没有任何变化，而Last-Modified却改变了，导致文件没法使用缓存

- 有可能存在服务器没有准确获取文件修改时间，或者与代理服务器时间不一致等情形


### 使用HTTP缓存好处
1. 减少了冗余的数据传输，节省了网费。
2. 缓解了服务器的压力， 大大提高了网站的性能
3. 加快了客户端加载网页的速度

### 不想使用缓存的几种方式
1. Ctrl + F5强制刷新，都会直接向服务器提取数据。
2. 按F5刷新或浏览器的刷新按钮，默认加上Cache-Control：max-age=0，即会走协商缓存。

### 总结
1、对于强制缓存，服务器通知浏览器一个缓存时间，在缓存时间内，下次请求，直接用缓存，不在时间内，执行协商缓存策略。
2、对于协商缓存，将缓存信息中的Etag和Last-Modified通过请求发送给服务器，由服务器校验，返回304状态码时，浏览器直接使用缓存。


### 浏览器首次和再次发送http请求的执行流程图
<img width="570" alt="image" src="https://user-images.githubusercontent.com/11763399/155348539-b77a40a3-6aca-42bc-8e1f-54ac9c8aa482.png">

<img width="797" alt="image" src="https://user-images.githubusercontent.com/11763399/155348430-8ee74097-96ea-4111-930b-556a3105a4ba.png">

## CDN
### CDN的优势
很明显：
- （1）CDN节点解决了跨运营商和跨地域访问的问题，访问延时大大降低；
- （2）大部分请求在CDN边缘节点完成，CDN起到了分流作用，减轻了源站的负载。
### CDN的缺点 
当网站更新时，如果CDN节点上数据没有及时更新，即便用户再浏览器使用Ctrl +F5的方式使浏览器端的缓存失效，也会因为CDN边缘节点没有同步最新数据而导致用户访问异常。
### CDN缓存策略 
CDN边缘节点缓存策略因服务商不同而不同，但一般都会遵循http标准协议，通过http响应头中的Cache-control: max-age的字段来设置CDN边缘节点数据缓存时间。

当客户端向CDN节点请求数据时，CDN节点会判断缓存数据是否过期，若缓存数据并没有过期，则直接将缓存数据返回给客户端；否则，CDN节点就会向源站发出回源请求，从源站拉取最新数据，更新本地缓存，并将最新数据返回给客户端。

CDN服务商一般会提供基于文件后缀、目录多个维度来指定CDN缓存时间，为用户提供更精细化的缓存管理。

CDN缓存时间会对“回源率”产生直接的影响。若CDN缓存时间较短，CDN边缘节点上的数据会经常失效，导致频繁回源，增加了源站的负载，同时也增大的访问延时；
若CDN缓存时间太长，会带来数据更新时间慢的问题。开发者需要增对特定的业务，来做特定的数据缓存时间管理。
### CDN缓存刷新
CDN边缘节点对开发者是透明的，相比于浏览器Ctrl+F5的强制刷新来使浏览器本地缓存失效，开发者可以通过CDN服务商提供的“刷新缓存”接口来达到清理CDN边缘节点缓存的目的。这样开发者在更新数据后，可以使用“刷新缓存”功能来强制CDN节点上的数据缓存过期，保证客户端在访问时，拉取到最新的数据。

> 浏览器第二次发送请求 => 浏览器缓存（强缓存、协商缓存） => cdn => 源服务器

## HTTP缓存决策
![image](https://user-images.githubusercontent.com/11763399/156971810-d67396c3-d4f1-49e5-b9f6-82fab7e2e4c0.png)

- 当我们的资源内容不可复用时，直接为 Cache-Control 设置 no-store，拒绝一切形式的缓存；
- 否则考虑是否每次都需要向服务器进行缓存有效确认，如果需要，那么设 Cache-Control 的值为 no-cache；
- 否则考虑该资源是否可以被代理服务器缓存，根据其结果决定是设置为 private 还是 public；
- 然后考虑该资源的过期时间，设置对应的 max-age 和 s-maxage 值；
- 最后，配置协商缓存需要用到的 Etag、Last-Modified 等参数。

## Memory Cache
MemoryCache，是指存在内存中的缓存。从优先级上来说，它是浏览器最先尝试去命中的一种缓存。从效率上来说，它是响应速度最快的一种缓存。

内存缓存是快的，也是“短命”的。它和渲染进程“生死相依”，当进程结束后，也就是 tab 关闭以后，内存里的数据也将不复存在。

那么哪些文件会被放入内存呢？

事实上，这个划分规则，一直以来是没有定论的。不过想想也可以理解，内存是有限的，很多时候需要先考虑即时呈现的内存余量，再根据具体的情况决定分配给内存和磁盘的资源量的比重——资源存放的位置具有一定的随机性。

虽然划分规则没有定论，但根据日常开发中观察的结果，包括我们开篇给大家展示的 Network 截图，我们至少可以总结出这样的规律：资源存不存内存，浏览器秉承的是“节约原则”。我们发现，Base64 格式的图片，几乎永远可以被塞进 memory cache，这可以视作浏览器为节省渲染开销的“自保行为”；此外，体积不大的 JS、CSS 文件，也有较大地被写入内存的几率——相比之下，较大的 JS、CSS 文件就没有这个待遇了，内存资源是有限的，它们往往被直接甩进磁盘。
## Service Worker Cache
Service Worker 是一种独立于主线程之外的 Javascript 线程。它脱离于浏览器窗体，因此无法直接访问 DOM。这样独立的个性使得 Service Worker 的“个人行为”无法干扰页面的性能，这个“幕后工作者”可以帮我们实现离线缓存、消息推送和网络代理等功能。我们借助 Service worker 实现的离线缓存就称为 Service Worker Cache。

Service Worker 的生命周期包括 install、active、working 三个阶段。一旦 Service Worker 被 install，它将始终存在，只会在 active 与 working 之间切换，除非我们主动终止它。这是它可以用来实现离线存储的重要先决条件。

下面我们就通过实战的方式，一起见识一下 Service Worker 如何为我们实现离线缓存（注意看注释）： 我们首先在入口文件中插入这样一段 JS 代码，用以判断和引入 Service Worker：
```javascript
window.navigator.serviceWorker.register('/test.js').then(
   function () {
      console.log('注册成功')
    }).catch(err => {
      console.error("注册失败")
    })
```
在 test.js 中，我们进行缓存的处理。假设我们需要缓存的文件分别是 test.html,test.css 和 test.js：
```javascript
// Service Worker会监听 install事件，我们在其对应的回调里可以实现初始化的逻辑  
self.addEventListener('install', event => {
  event.waitUntil(
    // 考虑到缓存也需要更新，open内传入的参数为缓存的版本号
    caches.open('test-v1').then(cache => {
      return cache.addAll([
        // 此处传入指定的需缓存的文件名
        '/test.html',
        '/test.css',
        '/test.js'
      ])
    })
  )
})

// Service Worker会监听所有的网络请求，网络请求的产生触发的是fetch事件，我们可以在其对应的监听函数中实现对请求的拦截，进而判断是否有对应到该请求的缓存，实现从Service Worker中取到缓存的目的
self.addEventListener('fetch', event => {
  event.respondWith(
    // 尝试匹配该请求对应的缓存值
    caches.match(event.request).then(res => {
      // 如果匹配到了，调用Server Worker缓存
      if (res) {
        return res;
      }
      // 如果没匹配到，向服务端发起这个资源请求
      return fetch(event.request).then(response => {
        if (!response || response.status !== 200) {
          return response;
        }
        // 请求成功的话，将请求缓存起来。
        caches.open('test-v1').then(function(cache) {
          cache.put(event.request, response);
        });
        return response.clone();
      });
    })
  );
});
```
PS：大家注意 Server Worker 对协议是有要求的，必须以 https 协议为前提。
