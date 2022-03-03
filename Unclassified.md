# 暂未分类内容

### 大文件下载暂停
- http range

### 性能优化
![image](https://user-images.githubusercontent.com/11763399/156503139-2cde551f-7ffc-4cd5-978d-0b47abb5f440.png)

- 指标
- 如何计算
    - performance fcp
    - 首屏 mutationObserver

### webworker


### ajax axios fetch区别

### .git
- hooks, pre-commit

### 错误监控
[参考链接](https://juejin.cn/post/6987681953424080926#heading-10)

### 图片转成base64优缺点
1. 优点

（1）base64格式的图片是文本格式，占用内存小，转换后的大小比例大概为1/3，降低了资源服务器的消耗；

（2）网页中使用base64格式的图片时，不用再请求服务器调用图片资源，减少了服务器访问次数。

2. 缺点

（1）base64格式的文本内容较多，存储在数据库中增大了数据库服务器的压力；

（2）网页加载图片虽然不用访问服务器了，但因为base64格式的内容太多，所以加载网页的速度会降低，可能会影响用户的体验。

（3）base64无法缓存，要缓存只能缓存包含base64的文件，比如js或者css，这比直接缓存图片要差很多，而且一般HTML改动比较频繁，所以等同于得不到缓存效益。
