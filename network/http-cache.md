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

