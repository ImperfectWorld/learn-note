# Rem布局原理

其实rem布局的本质是等比缩放 ,基于根节点font-size

# 实现

```javascript 
// 设置根节点fontsize
document.documentElement.style.fontSize = document.documentElement.clientWidth / 100 + 'px';
```

```
// 通过预处理sass/less 计算
$ue-width: 640; /* ue图的宽度 */

@function px2rem($px) {
  @return #{$px/$ue-width*100}rem;
}

p {
  width: px2rem(100);
}
```
