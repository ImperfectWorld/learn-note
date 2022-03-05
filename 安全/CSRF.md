# CSRF
> 冒充用户发起请求（在用户不知情的情况下）， 完成一些违背用户意愿的事情（如修改用户信息，删除评论等）。
![image](https://user-images.githubusercontent.com/11763399/155304440-bae516f9-4f84-4d6b-a4b2-7c8d58086fac.png)

- case
    举个例子，好友小A在银行存有一笔钱，输入用户名密码登录银行账户后，发送请求给xiaofan账户转888:
    ```bash
    http://bank.example.com./withdraw?account=xiaoA&amount=888&for=xiaonfan
    ```
    转账过程中， 小A不小心打开了一个新页面，进入了黑客（xiaohei）的网站，而黑客网站有如下html代码：
    ```html
    <html>
      <!--其他内容-->

      <img src=http://bank.example.com./withdraw?account=xiaoA&amount=888&for=xiaohei width='0' height='0'>

      <!--其他内容-->
    </html>
    ```
    这个模拟的img请求就会带上小A的session值， 成功的将888转到xiaohei的账户上。

- 攻击
    - 打开一个网站进行登录
    - 再打开一个钓鱼网站，钓鱼网站利用img.src=接口+参数, 可以执行接口请求
- 特点
    - 通常发生在第三方网站
    - 攻击者不能获取cookie等信息，只是使用
- 防御
    - 验证码：强制用户必须与应用进行交互，才能完成最终请求。此种方式能很好的遏制 CSRF，但是用户体验相对差。
    - 尽量使用 post ，限制 get 使用；上一个例子可见，get 太容易被拿来做 CSRF 攻击，但是 post 也并不是万无一失，攻击者只需要构造一个form就可以。
    - Referer check：请求来源限制，此种方法成本最低，但是并不能保证 100% 有效，因为服务器并不是什么时候都能取到 Referer，而且低版本的浏览器存在伪造 Referer 的风险。
    - token：token 验证的 CSRF 防御机制是公认最合适的方案。
