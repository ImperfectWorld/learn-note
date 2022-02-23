## http缓存
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
