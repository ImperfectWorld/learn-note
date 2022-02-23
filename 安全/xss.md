XSS （盗取cookie，劫持用户行为，钓鱼攻击，挂马）

- 攻击
    - 渲染过程中产生了一些预期外的指令
    - 访问页面时插入脚本执行
- 分类
    - 反射型 <script>alert('aa')</script>
    - 存储型（留言） input弹窗中，提交脚本信息。
    - 基于DOM
- 原因
    - 用户输入或url中的参数，没有做过滤，不合法参数到web服务器
- 防御
    - 对输入&URL过滤
    - 对输出编码
    - cookie 设置 http-only （js脚本无法读取cookie）
