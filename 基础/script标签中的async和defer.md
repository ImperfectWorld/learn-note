> async和defer，这两个属性使得script都不会阻塞DOM的渲染。


### defer

浏览器会异步的下载该文件并且不会影响到后续DOM的渲染;
defer脚本会在文档渲染完毕后，DOMContentLoaded事件调用前执行。

### async

会使得script脚本异步的加载并在允许的情况下执行;
async的执行，并不会按着script在页面中的顺序来执行，而是谁先加载完谁执行。

<img width="607" alt="image" src="https://user-images.githubusercontent.com/11763399/155492244-13950313-01da-4385-9fd6-755a383f3c73.png">

### 普通script
文档解析的过程中，如果遇到script脚本，就会停止页面的解析进行下载
（但是Chrome会做一个优化，如果遇到script脚本，会快速的查看后边有没有需要下载其他资源的，
如果有的话，会先下载那些资源，然后再进行下载script所对应的资源，这样能够节省一部分下载的时间 @Update: 2018-08-17）。

<img width="738" alt="image" src="https://user-images.githubusercontent.com/11763399/155492463-d6c952bf-e54b-49dd-bd08-4037afa53833.png">

### defer
文档解析时，遇到设置了defer的脚本，就会在后台进行下载，但是并不会阻止文档的渲染，当页面解析&渲染完毕后。
会等到所有的defer脚本加载完毕并按照顺序执行，执行完毕后会触发DOMContentLoaded事件。
<img width="728" alt="image" src="https://user-images.githubusercontent.com/11763399/155492550-f9cf479e-c9d3-4611-af26-b074ac8903f3.png">

### async
async脚本会在加载完毕后执行。
async脚本的加载不计入DOMContentLoaded事件统计，也就是说下图两种情况都是有可能发生的

<img width="747" alt="image" src="https://user-images.githubusercontent.com/11763399/155492854-79b26ed3-e829-4300-9e1d-076d9d6cc008.png">

### 推荐的应用场景
defer
如果你的脚本代码依赖于页面中的DOM元素（文档是否解析完毕），或者被其他脚本文件依赖。
例：
- 评论框
 
async
如果你的脚本并不关心页面中的DOM元素（文档是否解析完毕），并且也不会产生其他脚本需要的数据。
例：

- 百度统计

如果不太能确定的话，用defer总是会比async稳定。。。
