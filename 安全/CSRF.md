CSRF

![image](https://user-images.githubusercontent.com/11763399/155304440-bae516f9-4f84-4d6b-a4b2-7c8d58086fac.png)


- 攻击
    - 打开一个网站进行登录
    - 再打开一个钓鱼网站，钓鱼网站利用img.src=接口+参数, 可以执行接口请求
- 防御
    - POST （构造一个form也会攻击）
    - 加入验证码
    - 验证referer
    - Anti CSRF Token (请求加一个服务端随机生成的token，存储服务端，服务端验证，验证通过后销毁)
