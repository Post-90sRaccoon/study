# Flex 布局

```css
display:flex;
display:inline-flex
```

## css 中的宽度

### filil-available

* 类似 `<div>` ，自动填充满空间。可以用在 inline-block 身上。

### max-content

* 容器有足够的宽度和空间，所占的宽度。像 `white-space:nowrap`。如何处理空白，连续空白符被合并，文本内换行无效。

### min-content

* 采用内部元素最小宽度最大的元素的宽度，作为容器宽度。
* 最小宽度：
    * 替换元素，如图片，就是图片的宽度。
    * 文本: 一个中文的宽度值。
    * 文本有英文： 最小宽度是最长单词的长度。

### fit-content

* 与 float,absolut, inline-block 表现一致。

## Flex 容器上的 CSS 属性

### flex-direction

```css
flex-direction: row | row-reverse | column | column-reverse;
```

### flex-wrap

```css
flex-wrap: nowrap | wrap | wrap-reverse;
```

* nowrap: 默认值。
    * 如果 flex 子项 min-content 之和大于 flex 容器宽度，溢出。
    * 如果 flex 子项 min-content 之和小于 flex 容器宽度。
        * flex 子项 fit-content 宽度之和大于容器宽度，子项宽度收缩，正好到不溢出。
        * flex 子项 fit-content 宽度之和小于容器宽度，正常显示。

### flex-flow

```css
flex-flow: <‘flex-direction’> || <‘flex-wrap’>
```

### justify-content

* 主轴方向子项的对齐与分布。

```css
justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
```

### align-items

* flex 的子项们相对 flex 容器在垂直方向的对齐方式。

```css
align-items: stretch | flex-start | flex-end | center | baseline;
```

* stretch：默认值。子项没指定高度，拉伸。
* baseline: 基线(字母x带下边缘)对齐。

### align-content

* 交叉轴每一行/列 flex 元素的对齐和分布方式。只有一行/l列，不生效。

```css
align-content: stretch | flex-start | flex-end | center | space-between | space-around | space-evenly;
```

* stretch 每一行/列 等比拉伸，两行，每一行高度 50%。

### align-items 和 align-content 的区别

* align-content 分配交叉轴剩余空间，
    * 多行时要有剩余空间（flex 容器要有高度）。
    * 单行时还要 `flex-wrap: wrap`。
* align-items: 每个 flex 行 的布局，flex 行由本行最高的 flex 元素撑起。、

### flex-basis

* 不设置 width 或 height，设置 flex-basis。

### flex

* flex: flex-grow, flex-shrink, flex-basis。
* 默认值 0 1 auto。
* none: 0 0 auto。
* auto: 1  1 auto。

```css
flex: none | auto | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
```

* |  但管道符，表示排他。
* []  表示支持的属性值在这个范围内
* ？ 表示 0 个 或 1 个
* || 或者的意思。表示前后可以分开独立合法使用。

```css
flex:1
/* flex: 1 1 0 */
flex: 100px
/* flex 1 1 100px */
```

* 使用 flex 属性可以不用 min-widht min-height
* flex-basis 优先级大于 width。
* width 小于内容最小宽度，内容溢出。flex-basis小于内容最小宽度，以最小宽度为准。

## Flex 布局的常见问题

### 最后一行左对齐

#### 最后一行列数固定

* 用 margin 模拟 space-between

```css
:not(:nth-child(xn)) {
  margin-right: calc(?% / x)
}
```

* 根据最后一个元动态 margin

```css
last-child:nth-child(xn - 1)
last-child:nth-child(xn - 2)
```

#### 每一子项宽度不固定

* 让最后一行左对齐即可

```css
:last-child {
  margin-right: auto;
}
```

* 创建伪元素并设置flex:auto或flex:1

```css
.container::after {
  content: '';
  flex: auto;    /* 或者flex: 1 */
}
```

#### 每一行列数不固定

* 这个布局最多 7 列，就用 7 个空白标签填充。最多 10 列，用 10个 空白标签。

```html
<i></i><i></i><i></i><i></i><i></i><i></i><i></i>
```

```css
.container > i {
    width: 100px;
    margin-right: 10px;
}
```

* 由于`<i>`元素高度为0，因此，并不会影响垂直方向上的布局呈现。

#### 列数不固定 HTML又不能调整

* 使用 grid 布局。

### flex-end overflow 无法滚动

* 只有容器下方（或右侧）内容有多余，才滚动。
* 设置第一个子项设置起始方向的 `margin` 属性值为 `auto`。
