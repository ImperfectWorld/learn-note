# 什么是webp
是一种同时提供了有损压缩与无损压缩（可逆压缩）的图片文件格式
# webp优势
### 优势
它具有更优的图像数据压缩算法，能带来`更小的图片体积`，而且拥有肉眼识别无差异的图像质量；同时具备了`无损和有损`的压缩模式、Alpha 透明以及动画的特性，在 JPEG 和 PNG 上的转化效果都相当优秀、稳定和统一。
### 劣势
兼容性不好，IE都不支持，苹果iOS 14后支持，安卓基本都支持
![image](https://user-images.githubusercontent.com/11763399/197341769-fee1d1ae-937c-4e70-9d6e-caa3c69ccb48.png)

# 如何使用
### HTML中
借助` <picture>` 标签自适应加载的特性，如下，WebP 格式放在 <source> 标签，用JPEG、PNG等通用格式做兜底。
``` HTML
// 方法一：
<picture>
    <source srcset="@/assets/images/yk.webp" type="image/webp" />
    <img decoding="async" loading="lazy" src="@/assets/images/yk.jpg" alt="" />
</picture>
<img src="@/assets/images/yk.jpg" alt="" />

// 方法二: 利用error属性
<img
 src="'./image.webp"
 onError={({ currentTarget = {} }: any) => {
   currentTarget.onerror = null;
   currentTarget.src = './image.png';
 }}
/>
```
> 小知识点1：`decoding`： 给html标签添加`decoding`属性用于告诉浏览器使用何种方式解析图像数据，async异步解码图片，降低图片资源优先级，优先加载其他元素.
  ![image](https://user-images.githubusercontent.com/11763399/197316638-c7141afd-1b23-42dc-b587-d10642d76284.png)

> 小知识点2：`loading`：它的值会告诉浏览器不在可视视口内的图片该如何加载；
  loading:lazy => 告诉用户代理推迟图片加载直到浏览器认为其需要立即加载时才去加载。
  例如，如果用户正在往下滚动页面，值为 lazy 会导致图片仅在马上要出现在 可视视口中时开始加载。
  注意：lazy后加载图片，会导致重绘，所以最好提前设置元素宽高，避免页面重新渲染。

### vue-directive  指令
```
// 注册一个全局自定义指令 `v-webp`
const vWebp = {
    mounted: (el, binding, vnode) => {
        let src = el.src;
        el.src = src + '.webp';
        el.onerror = () => {
            el.src = src;
        };
    },
};

<img v-webp src="require('../assets/banner.png')" alt="">

const vWebpBg = {
    mounted: (el, binding, vnode) => {
        let str = el.getAttribute('class').split(' ');
        str.push('webpa');
        el.setAttribute('class', str.join(' '));
    },
};
<div class="banner" v-webp-bg></div>
// webpBg指令给节点添加一个className，配合css设置

.webpbg(@url) {
  background-image: url(@url);
  &.webpa  {
     background-image: url('@{url}_.webp');
  }
}

.banner{
  width:300px;
  height: 300px;
  .webpbg("../assets/banner.png")
}
```
### 判断是否支持
```
// 利用canvas
var isSupportWebp = function () {
     try {
          return document.createElement('canvas').toDataURL('image/webp', 0.5).indexOf('data:image/webp') === 0;
     } catch(err) {
          return false;
     }
}
// 通过加载一张 webp 图片进行判断
// Google官方文档是这样处理的（先加载一个WebP图片，如果能获取到图片的宽度和高度，就说明是支持WebP的，反之则不支持）
function check_webp_feature(feature, callback) {
    var kTestImages = {
        lossy: "UklGRiIAAABXRUJQVlA4IBYAAAAwAQCdASoBAAEADsD+JaQAA3AAAAAA",
        lossless: "UklGRhoAAABXRUJQVlA4TA0AAAAvAAAAEAcQERGIiP4HAA==",
        alpha: "UklGRkoAAABXRUJQVlA4WAoAAAAQAAAAAAAAAAAAQUxQSAwAAAARBxAR/Q9ERP8DAABWUDggGAAAABQBAJ0BKgEAAQAAAP4AAA3AAP7mtQAAAA==",
        animation: "UklGRlIAAABXRUJQVlA4WAoAAAASAAAAAAAAAAAAQU5JTQYAAAD/////AABBTk1GJgAAAAAAAAAAAAAAAAAAAGQAAABWUDhMDQAAAC8AAAAQBxAREYiI/gcA"
    };
    var img = new Image();
    img.onload = function () {
        var result = (img.width > 0) && (img.height > 0);
        callback(feature, result);
    };
    img.onerror = function () {
        callback(feature, false);
    };
    img.src = "data:image/webp;base64," + kTestImages[feature];
}

```
