# Vue Basic

## 语法

### `v-once`

```html
<a v-once>{{}}</ a>
// 插值处不会更新
```

### `v-html`

```html
<span v-html="rawhtml"> 
// 输出真正的 html 而不是解析
```

### 模板表达式

* 可以用 js 表达式，但只能单个表达式。
* 模板表达式只能访问全局变量的一个白名单。如 `Math`

### 动态参数

* `<a v-bind:[attributeName]="url">`
* 动态参数期望一个字符串，异常情况下值为 `null` 用于移除绑定。
* `HMLT`属性里空格和引号无效，避免使用大写字符来命名键名。

### 简写

* SPA 中`v-`前缀 可以简写。

### 计算属性

* 处理复杂逻辑。
* 计算属性结果可通过 `vm.[计算属性名]`的`getter`拿到。

### 计算属性 vs 方法

* 计算属性基于响应式依赖缓存。只在相关响应式依赖发生改变时他们才重新求值。所以必须依赖响应式更新的值。(Date.now 不会更新)
* 每次触发重新渲染时，调用方法总会再次执行函数。

### 计算属性 vs 侦听属性

* 侦听属性：观察 Vue  实例上的数据变动。
* 计算属性也提供 `setter`。

### 侦听器

* 当数据变化时执行异步或开销较大的操作是使用侦听器。
* 如果侦听的 `data`发生变化，侦听器函数就会执行。

### 绑定 Class 与 对象

* 可以用对象、可以绑定计算属性、可以用数组语法，数组语法里也可以用对象语法。
* 在自定义组件上使用 `class property`。`class` 将添加到该组件的根元素上面。

### v-if 和 v-else

* `v-if`、`v-else`  和 `v-else-if`。
* 配合 `<template>` 标签使用。

### Key

* 用 `key` 管理可复用的元素。

###  v-if 和 v-show

* `v-if` 销毁重建，包括事件的监听器。惰性，初始条件为假，什么也不做，第一次为真，才会渲染。
* `v-show` 元素总是渲染 `display` 切换。

### v-for

* 可以遍历对象。遍历对象时三个参数，值，键，索引。
* 与 key 连用。
* 数组更新检测。`push pop shift unshift splice sort reverse`。
* 显示过滤排序后的结果。使用计算属性。
* `v-for` 循环计算属性不适用，可以用方法。
* `v-for` 优先级比 `v-if` 更高。想渲染部分节点，十分有用。

### 事件

* 内联处理器访问 `Dom` 原生事件传 `$event`。
* 事件修饰符。 `.stop、.prevent、.capture、.self、.once、.passive`
* `.once` 事件只触发一次，不仅对原生 `dom`起作用，还可以被应用到自定义组件事件上。
* `.self` 事件自身触发。不是内部元素触发。
* `<div v-on:scroll.passive="onScroll">...</div>` 滚动立刻触发 而不是等待 onScroll 完成。
* `v-on:keyup.enter = "submit"` `keyup.page-down`
* `keyup.ctrl` 事件触发必须处于按下状态。
* `.exact` 精确的修饰符组合触发事件。

### 表单输入绑定

* `v-model` 忽略表单初始值，总是将 `Vue` 实例的数据作为数据来源。
    * text 和 textarea 中 `value` 属性 `input` 事件。
    * checkbox 和 radio 中 `checked` 属性和 `change` 事件。
    * select 中 `value` 属性 `change` 事件。

#### 值绑定

* checkbox，radio 和 select，v-model 绑定的通常是静态字符串。

```html
<input type="radio" v-model="picked" value="a">
<!-- 选中时 picked 是字符串 a
想绑定到动态 property 上 需要使用 bind, 这时 :value 可以不是字符串。
选中时 value 为 a，value 为 a 时才被选中。
-->

<input type="radio" v-model="pick" v-bind:value="a">
<!-- 选中时 vm.pick === vm.a -->
```

#### 修饰符

* `.trim`、`.lazy`、`.number` 修饰符

### 组件基础 

#### 通过 Prop 向子组件传递数据

* `props` 传递，使用 `v-bind` 动态传递 `props`。

#### 自定义事件

* 父组件绑定自定义事件`v-on:event-name`，子组件行内用 `$emit('event-name')`,触发自定义事件。方法里可以用 `this.$emit`
* 子组件 `$emit('enlarge-text,value')` 父组件 `@enlarge-text=“fontSize += $event”`，如果父组件的事件处理函数是方法，值为第一个参数。

#### 自定义组件使用 `v-model`

```html
<input v-model="searchText">
<!-- 相当于 -->
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>

<!-- 用在组件时 -->
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>

<!-- 为了让它正常工作，这个组件内的 <input> 必须：
将其 value attribute 绑定到一个名叫 value 的 prop 上
在其 input 事件被触发时，将新的值通过自定义的 input 事件抛出
-->
<script>
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
 </script>
```

#### 动态组件

* 通过 `<component>` 元素和 `is` attribute 实现。

```html
<component v-bind:is="currentTabComponent"></component>
```

* `is` 可以作用于 `html` 元素。 但这些元素将被视为组件，这意味着所有 attribute 都会作为 DOM attribute 绑定。对于像 value 这样的属性，需要使用 `.prop` 修饰器。