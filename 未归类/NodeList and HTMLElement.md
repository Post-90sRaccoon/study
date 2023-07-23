# NodeList and HTMLElement

## DOM 结点和 HTML 元素结点

### DOM 结点

* `DOM` 结点是文档对象模型（DOM）中的基本单位，代表文档中的所有元素、属性、文本和注释。
* `DOM` 结点是树状结构的一部分，每个 `DOM` 结点都有一个父结点（除了根结点）和零个或多个子结点。
* 在 `DOM` 中，所有的内容都被表示为结点，例如元素节点、文本节点、注释节点等。

### HTML 元素结点

* `HTML` 元素节点是 `DOM` 结点的一种特殊类型，它代表 `HTML` 文档中的 `HTML` 元素。
* 在 `HTML` 文档中，`HTML` 元素是由开始标签、内容和结束标签组成的，例如`<div>...</div>`、`<p>...</p>`等。
* 每个 `HTML` 元素在 `DOM` 中都被表示为一个元素节点，它是 `DOM` 树中的一个结点，具有父结点和可能的子结点。

```html
<div id="myDiv">Hello, world!</div>
```

在上述示例中，`<div>` 元素是一个 `HTML` 元素节点，它在 `DOM` 中被表示为一个元素节点。`"Hello, world!"`是一个文本节点，也是 `DOM`的一个结点，但它不是 `HTML` 元素节点。

## NodeList 和 HTMLElement 的区别

### NodeList

* `NodeList` 是一个类数组对象，它表示 `DOM` 中的一组节点集合，通常是由查询 `DOM` 元素的方法（例如`querySelectorAll()`）返回的结果。
* `NodeList` 对象类似于数组，它包含了一组有序的节点，但它不是数组，没有数组的方法（如`map()`、`forEach()`等），但可以通过索引来访问其中的元素。
* `NodeList` 对象是动态的，也就是说当 `DOM` 结构发生变化时，`NodeList` 会自动更新。