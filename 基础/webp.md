# 什么是webp

# webp优势

# 如何使用
## HTML中
借助 <picture> 标签自适应加载的特性，如下，WebP 格式放在 <source> 标签，用JPEG、PNG等通用格式做兜底。
``` HTML
<picture>
    <source srcset="@/assets/images/yk.webp" type="image/webp" />
    <img decoding="async" loading="lazy" src="@/assets/images/yk.jpg" alt="" />
</picture>
<img src="@/assets/images/yk.jpg" alt="" />
```
> 小知识点1：`decoding`： 给html标签添加`decoding`属性用于告诉浏览器使用何种方式解析图像数据，async异步解码图片，降低图片资源优先级，优先加载其他元素.
  ![image](https://user-images.githubusercontent.com/11763399/197316638-c7141afd-1b23-42dc-b587-d10642d76284.png)

> 小知识点2：`loading`：它的值会告诉浏览器不在可视视口内的图片该如何加载；
  loading:lazy => 告诉用户代理推迟图片加载直到浏览器认为其需要立即加载时才去加载。例如，如果用户正在往下滚动页面，值为 lazy 会导致图片仅在马上要出现在 可视视口中时开始加载。
  
# 小米粒
