AMD 规范在这里：https://github.com/amdjs/amdjs-api/wiki/AMD

CMD 规范在这里：https://github.com/seajs/seajs/issues/242

AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。

CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。类似的还有 CommonJS Modules/2.0 规范，是 BravoJS 在推广过程中对模块定义的规范化产出。还有不少⋯⋯
这些规范的目的都是为了 JavaScript 的模块化开发，特别是在浏览器端的。

> 目前这些规范的实现都能达成浏览器端模块化开发的目的。

区别：
- 对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.
- CMD 推崇依赖就近，AMD 推崇依赖前置。

看代码：

```bash
// CMD
define(function(require, exports, module) {
  var a = require('./a')
  a.doSomething()
  // 此处略去 100 行
  var b = require('./b') // 依赖可以就近书写
  b.doSomething()
  // ... 
})
// AMD 默认推荐的是
define(['./a', './b'], function(a, b) {  // 依赖必须一开始就写好
  a.doSomething()
  // 此处略去 100 行
  b.doSomething()
  ...
})
```

- AMD 的 API 默认是一个当多个用，CMD 的 API 严格区分，推崇职责单一。

比如 AMD 里，require 分全局 require 和局部 require，都叫 require。
CMD 里，没有全局 require，而是根据模块系统的完备性，提供 seajs.use 来实现模块系统的加载启动。
CMD 里，每个 API 都简单纯粹。


作者：玉伯
链接：https://www.zhihu.com/question/20351507/answer/14859415
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
