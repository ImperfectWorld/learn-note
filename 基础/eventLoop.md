# 介绍
**Event Loop** 即事件循环，是指浏览器或Node的一种解决javaScript单线程运行时不会阻塞的一种机制，也就是我们经常使用异步的原理。

## Event Loop
在JavaScript中，任务被分为两种，一种宏任务（MacroTask）也叫Task，一种叫微任务（MicroTask）。

### MacroTask（宏任务）
script全部代码、setTimeout、setInterval、I/O、UI Rendering。


### MicroTask（微任务）
Process.nextTick（Node独有）、Promise、MutationObserver
