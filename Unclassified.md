# 暂未分类内容

## 原型与原型链

### 大文件上传
https://zhuanlan.zhihu.com/p/68271019

### 大文件下载暂停
- http range

content-type: multipart/form-data
利用表单长传文件的时候，必须让 form 的 enctyped 等于这个值；这个格式一般是用来上传文件的，各大服务端语言对他也有着良好的支持

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

### 鉴权

### nodejs作为BFF的优缺点
- 页面服务。自从前后端分离以来，前端的页面服务就由独立的服务来提供。BFF可以提供页面服务，在这个基础上还可以做SSR，大幅提升页面加载性能。此外，还可以将逻辑数据传递到基础页面上，为前端注入一些基础配置或者状态，在某些场景下是很有用的。
- 网关。可以参考上文提到的网关的各项功能，在BFF层常见的比如配合SSO统一登陆服务识别并透传用户身份，还有流控、防攻击、操作日志、跨域接口转发等。
- 聚合层。这部分是BFF的重点职责，主要是对服务端提供接口数据的聚合和加工，下文会重点介绍聚合层的适应场景。
其他。除了以上三个主要场景，还有一些独立的轻量服务适合BFF来做，比如图片上传和WebSocket服务。


### nodejs可以高并发的原因

### nodejs线程如何进行通信

### 前端微服务
https://juejin.cn/post/6917848537686507533
https://juejin.cn/post/6973111766767108103
https://www.bilibili.com/video/BV1H34y117fe?from=search&seid=5947971260081305233&spm_id_from=333.337.0.0

### content-type 类型
Content-Type: application/json ： 请求体中的数据会以json字符串的形式发送到后端
Content-Type: application/x-www-form-urlencoded：请求体中的数据会以普通表单形式（键值对）发送到后端
Content-Type: multipart/form-data： 它会将请求体的数据处理为一条消息，以标签为单元，用分隔符分开。既可以上传键值对，也可以上传文件。

### 变量/作用域/闭包
定义：简单来说作用域就是变量与函数的可访问范围，由当前环境与上层环境的一系列变量对象组成
1.全局作用域：代码在程序的任何地方都能被访问，window 对象的内置属性都拥有全局作用域。
2.函数作用域：在固定的代码片段才能被访问
作用：作用域最大的用处就是隔离变量，不同作用域下同名变量不会有冲突。

### 岛屿问题
