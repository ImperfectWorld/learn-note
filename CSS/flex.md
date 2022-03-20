# Flex 布局



## 容器属性

- flex-direction   主轴方向（即项目的排列方向）

  ```css
  flex-direction: row | row-reverse | column | column-reverse;
  ```

- flex-wrap   如果一条轴线排不下，如何换行

  ```css
  flex-wrap: nowrap | wrap | wrap-reverse;
  ```

- flex-flow   flex-direction 和 flex-wrap 简写

- justify-content   项目在主轴上的对齐方式

  ```css
  justify-content: flex-start | flex-end | center | space-between | space-around
  ```

- align-items    项目在交叉轴上对齐方式

  ```css
  align-items: flex-start | flex-end | center | baseline | stretch
  ```

- align-content    多跟轴线的对齐方式

  ```css
  align-content: flex-start | flex-end | center | space-between | space-around | stretch;
  ```



## 项目的属性

- order    项目的排列顺序，数值越小，排列越靠前
- flex-grow    项目放大的比例

```css
flex-grow: 1; // 平均分布，默认是0
```

- flex-shrink    项目缩小的比例

```css
flex-grow: 1 // 空间不足时，都等比例缩小，负值无效
```

- flex-basis    分配多余空间之前，项目占据的主轴空间
- flex    flex-grow, flex-shrink, flex-basis的缩写 0 1 auto
- align-self    修改单个项目的对齐方式

```css
align-self: auto | flex-start | flex-end | center | baseline | stretch;
```

