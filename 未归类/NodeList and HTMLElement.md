# NodeList，HTMLCollection and HTMLElement

## DOM

* `DOM` 节点是文档对象模型（DOM）中的基本单位，代表文档中的所有元素、属性、文本和注释。
* `DOM` 节点是树状结构的一部分，每个 `DOM` 节点都有一个父节点（除了根节点）和零个或多个子节点。
* 在 `DOM` 中，所有的内容都被表示为节点，例如元素节点、文本节点、注释节点等。

## Node 与 Element

* Element 是 `nodeType` 为 `Node.Element_Node` 的 Node。

## HTML Element

* `HTML` 元素节点是 `DOM` 节点的一种特殊类型，它代表 `HTML` 文档中的 `HTML` 元素。

```html
<div id="myDiv">Hello, world!</div>
```

在上述示例中，`<div>` 元素是一个 `HTML` 元素节点，它在 `DOM` 中被表示为一个元素节点。`"Hello, world!"`是一个文本节点，也是 `DOM`的一个节点，但它不是 `HTML` 元素节点。

## NodeList

* `NodeList` 它表示 `DOM` 中的一组节点集合。
* `NodeList` 类数组，没有数组的方法（如`map()`），但可以通过索引来访问其中的元素。
* `NodeList` 对象是动态的，也就是说当 `DOM` 结构发生变化时，`NodeList` 会自动更新。

## HTMLCollection

* `HTMLElement` 的集合，同样是类数组。



[Node 与 Element 的区别](https://zhuanlan.zhihu.com/p/165422508)

[Node 与 Element](https://juejin.cn/post/7066778860024496165)