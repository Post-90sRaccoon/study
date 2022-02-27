#  Vue Basic

## 语法

### `v-once`

```html
<a v-once>{{}}</ a>
// 插值处不会更新
```

* 除非非常留意渲染变慢了，不然完全没有必要。

### 模板表达式

* 可以用 js 表达式，但只能单个表达式。
* 模板表达式只能访问全局变量的一个白名单。如 `Math`

### 动态参数

* `<a v-bind:[attributeName]="url">`
* 动态参数期望一个字符串，异常情况下值为 `null` 用于移除绑定。
* `HMLT`属性里空格和引号无效，避免使用大写字符来命名键名。因为浏览器会把 attribute 名全部强制转为小写。

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

* 当数据变化时要执行异步或开销较大的操作是使用侦听器。
* 如果侦听的 `data`发生变化，侦听器函数就会执行。

### 绑定 Class 与 对象

* 可以用对象动态切换 class、可以绑定计算属性、可以用数组语法，数组语法里可以用三元表达式，也可以用对象语法。
* 在自定义组件上使用 `class property`。`class` 将添加到该组件的根元素上面。

### v-if 和 v-else

* `v-if`、`v-else`  和 `v-else-if`。
* 配合 `<template>` 标签使用。`v-else` 不行。

### Key

* 用 `key` 管理可复用的元素。

###  v-if 和 v-show

* `v-if` 销毁重建，包括事件的监听器。惰性，初始条件为假，什么也不做，第一次为真，才会渲染。
* `v-show` 元素总是渲染 `display` 切换。

### v-for

* 可以遍历对象。遍历对象时三个参数，值，键，索引。
* 与 key 连用。不使用 key,数据顺序改变，不移动 dom,就地更新。不给 key 就是这种默认行为。**key 要用字符串或数值类型** 。
* 数组更新检测。`push pop shift unshift splice sort reverse`。
* 显示过滤排序后的结果。使用计算属性。
* `v-for` 里可以用方法。

     ```javascript
     <li v-for="n in even(set)">{{ n }}</li>
     ```

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
* `v-model` 不会再输入法组合文字时更新，input 事件会。
* `v-model` 表达式的初始值未能匹配任何选项，select 元素会处于未选中状态。ios 下用户无法选择第一个选项，推荐提供一个值为空的禁用选项。

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

#### 修饰符 v-model

* `.trim`、`.lazy`、`.number` 修饰符

### 组件基础

#### 通过 Prop 向子组件传递数据

* `props` 传递，使用 `v-bind` 动态传递 `props`。
* 可以 `v-bind="obj` 传入一个对象的所有 property。
* 单向数据流，不应该改变子组件的 prop。对象和数组是引用传入，子组件中更改，会影响父组件状态。
* 对象或数组默认值必须从一个工厂函数获取。

```javascript
propE: {
  type: Object,
  default: function () {
    return { message: 'hello' }
  }
}
```

* prop 会在实例创建之前验证，所以实例在 default 和 validator 中无法访问。
* type 可以是自定义构造函数。
* 非 prop 的 attribute 会被添加到子组件根元素上。
* 禁用 attribute 继承(不会影响 style 和 class 的绑定)，使用实例的 `$attrs`。 

#### 自定义事件

* 父组件绑定自定义事件`v-on:event-name`，子组件行内用 `$emit('event-name')`,触发自定义事件。方法里可以用 `this.$emit`
* 子组件 `$emit('enlarge-text,value')` 父组件 `@enlarge-text=“fontSize += $event”`，如果父组件的事件处理函数是方法，值为第一个参数。

#### 自定义组件使用 `v-model`

* model 选项

```javascript
model: {
 prop: 'checked',
 event: 'change'
},
```

```html
<input v-model="search
                Text">
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

#### 自定义组件的根元素绑定原生事件

* 使用`.native` 修饰符。
* `$listeners` 类似`$attrs`

### .sync 修饰符

* 对一个 prop 双向绑定，无法真正双向绑定。以 update:ppropName 的方法取而代之。

```html
this.$emit('update:title', newTitle)

<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>

<!-- 等价于 -->
<text-document v-bind:title.sync="doc.title"></text-document>
```

* 使用 `.sync` 不能用表达式，类似 v-model。 

### .sync 和 v-model

* v-model 一个组件上只能有一个。.sync 可以用多个。
* 作用
    * `v-model="a"`  `<comp :value="a" @input="a=$event"></comp>`
    * `:title.sync="a"` `<comp :title="a" @update:title="a=$event"></comp>`

### 插槽

#### 编译作用域

* 插槽访问他写的位置的作用域，而不是插入的子组件的作用域。父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的.

#### 后备内容

```html
<slot>a</slot>
```

* 默认是 a , 用内容内容替换 a。

#### 具名插槽

* 多个插槽，通过 slot 的 attribute `name` 来区分。
* 向具名插槽提供内容通过 template 的 `v-slot:name`,不符合此条件的都插入到默认插槽中。

#### 作用域插槽

* 让插槽内容能够访问子组件的数据。
* 为了让数据在父级插槽可用，使用 slot 的 `v-bind:data="value"`。
* 父级的 template 使用`v-slot:name="slotProps"` `slotProps.data`。
* v-slot 只能在 template 上添加。除非被提供的内容只有默认插槽时，可以添加在组件上。
* 可以解构 slotProps `v-slot:name={ data: person }`。包括赋默认值。

#### 动态插槽名

* 同动态参数名 `v-slot:[name]`

#### 具名插槽缩写

* 父级 template `#name`，`#name={ user }`。

###  动态组件和异步组件

#### 动态组件

* 通过 `<component>` 元素和 `is` attribute 实现。

```html
<component v-bind:is="currentTabComponent"></component>
```

* `is` 可以作用于 `html` 元素。 但这些元素将被视为组件，这意味着所有 attribute 都会作为 DOM attribute 绑定。对于像 value 这样的 property，需要使用 `.prop` 修饰器。
* keep-alive 需要组件有自己的名字组件的 name。

#### 异步组件

```javascript
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 向 `resolve` 回调传递组件定义
    resolve(
      {
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```

* 与 webpack 的 code-splitting 功能配合使用

```javascript
Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 `require` 语法将会告诉 webpack
  // 自动将你的构建代码切割成多个包，这些包
  // 会通过 Ajax 请求加载
  require(['./my-async-component'], resolve)
})
```

* 也可以工厂函数返回一个 promise

```javascript
Vue.component(
  'async-webpack-example',
  // 这个动态导入会返回一个 `Promise` 对象。
  () => import('./my-async-component')
)
```

* 局部注册时，直接提供一个返回  Promise 的函数

```javascript
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})
```

##### 异步组件处理加载状态

* 异步组件工厂函数返回一个对象，有相应配置项，详见文档。

#### 处理边界情况

##### 访问元素 & 组件

* 子组件可以通过 `this.$root` 访问根实例。
* 子组件通过 `this.$parent` 访问父级组件实例。
* 使用 `ref` 访问子组件或者子元素。通过 `this.$refs.xx` 访问。**`$refs` 组件渲染后生效，不是响应式。**
* 依赖注入(非响应式的)，嵌套层级深子组件无法通过 `this.$parent` 访问，也不需要暴露整个父级实例。

```html
<google-map>
  <google-map-region>
    <google-map-markers></google-map-markers>
  </google-map-region>
</google-map>
```

* google-map 

```javascript
provide: function () {
  return {
    getMap: this.getMap
  }
}
```

* 后代组件

```javascript
inject: ['getMap']
```

##### 程序化的事件监听器

* `$on`,`$once`,`$off`。

```javascript
mounted: function () {
  var picker = new Pikaday({
    field: this.$refs.input,
    format: 'YYYY-MM-DD'
  })

  this.$once('hook:beforeDestroy', function () {
    picker.destroy()
  })
}
// 这样不需要把 picker 挂载到实例上。
```

##### 递归组件

* 不能无限递归，要用终止条件，如最后得到一个 false 的 `v-if`。

##### 组件间的循环引用

* 全局注册可以。

* 使用模块系统时，产生悖论的子组件是 `tree-folder-contents`。

    * beforeCreate 时去注册他

        ```javascript
        beforeCreate: function () {
          this.$options.components.TreeFolderContents = require('./tree-folder-contents.vue').default
        }
        ```

    * 使用 webpack 异步 import

    ```javascript
    components: {
      TreeFolderContents: () => import('./tree-folder-contents.vue')
    }
    ```

##### 模板定义的替代品

* 内联模板。
* X-Template。

##### 控制更新

* 强制更新。`$forceUpdate`。

#### 混入

* 一个混入对象包含任意组件 option。

```javascript
const myMixin = {}
const Component = Vue.extend({ mixins: [myMixin] })
```

* 冲突时组件数据优先。
* 钩子函数合并为数组都调用，混入对象的钩子在自身钩子前调用。
* 值为对象，合并为一个对象。
* 可以自定义选项合并策略。

#### 自定义指令

* 局部注册 `directives:`

```javascript
// v-focus
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    el.focus()
  }
})
```

##### 钩子函数

* `bind` 指令第一次绑定到元素时调用。一次性的初始化设置。
* `inserted` 被绑定元素插入父节点时调用。 (仅保证父节点存在，但不一定已被插入文档中)
* `updated` 所有组件的 VNode 更新时调用，也可能是发生在更新之前。
* `componentUpdated` 指令所在组件的 VNode 及其子 VNode 全部更新后调用。
* `unbind` 只调用一次，指令与元素解绑时调用。

##### 参数

* `el`: 绑定元素，可操作 DOM。
* `binding` 一个对象
    * `name` 指令名
    * `value` 指令的绑定值 `v-xx=1`
    * `oldvalue` 指令绑定的前一个值
    * `expression` 字符串形式的的指令表达式 `v-xx=“'1+1'”`
    * `arg` 传给执行的参数。`v-xx:foo`
    * `modifiers` 执行修饰符，一个对象。
* `VNode` Vue 编译的虚拟节点
* `oldVnode` 上一个虚拟节点
* 除了 `el` ，其他参数是只读的，钩子共享数据可以通过 dataset。

##### 动态指令参数

* `v-xx:[argument]="value"`

##### 函数简写

* `bind` 和 `update` 触发相同行为，直接写 function 即可.

##### 对象字面量

* 如果指令需要多个值，可以传入 JavaScript 对象字面量。`v-xx="{a:"1“,b:"2"}"`

#### 渲染函数和 JSX

##### 基础

```javascript
Vue.component('anchored-heading', {
  render: function (createElement) {
    return createElement(
      'h' + this.level,   // 标签名称
      this.$slots.default // 子节点数组
    )
  },
  props: {
    level: {
      type: Number,
      required: true
    }
  }
})
```

##### 结点，树和虚拟 DOM

* 虚拟 DOM `createNodeDescription`
* `createElement`

```javascript
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签名、组件选项对象，或者
  // resolve 了上述任何一种的一个 async 函数。必填项。
  'div',

  // {Object}
  // 一个与模板中 attribute 对应的数据对象。可选。
  {
    // 详情见文档
  },

  // {String | Array}
  // 子级虚拟节点 (VNodes)，由 `createElement()` 构建而成，
  // 也可以使用字符串来生成“文本虚拟节点”。可选。
  [
    '先写一些文字',
    createElement('h1', '一则头条'),
    createElement(MyComponent, {
      props: {
        someProp: 'foobar'
      }
    })
  ]
)
```

* 组件树的 VNode 必须是唯一的。重复用工厂函数实现。

```javascript
render: function (createElement) {
  return createElement('div',
    Array.apply(null, { length: 20 }).map(function () {
      return createElement('p', 'hi')
    })
  )
}

// apply 第二个参数可以接类数组对象 [undefined,undefined，...]
// Array(20) => [empty,empty,...]
// map 会跳过 empty
// es6 Array.from({ length: 2 }) Array(2).fill(null)
```

##### 替代模板功能

* 详见文档

#### 插件

* 添加全局功能。

##### 使用

* `new Vue()` 前使用 `Vue.use(plugin, { someOptions: true })`  

* `Vue.use(plugin)` 调用了 `plugin.install(Vue)`

*  `CommoJS `环境中，必须显示调用 `Vue.use()`

##### 开发

* 应暴露一个 `install` 方法

```javascript
// 第一个参数是 Vue 构造器，第二个是可选选项对象
MyPlugin.install = function (Vue, option) {
  Vue.method = function() {} // 添加全局方法 或 property
  Vue.directive('') // 添加全局资源
  Vue.mixin({}) // 注入组件选项
  Vue.prototype.$myMethod = function() {} // 添加实例方法
}
```

##### 过滤器

* 文本格式化。可用在双花括号差值和 `v-bind` 表达式。

```html
{{ message | formatMessage }}
<div :id="rawId | formatId"></div>
```

```javascript
// 本地
filters: {
  xx: function(value) {}
}

// 全局
Vue.filter('capitalize', function(value) { reutrn xxx })

{{ message | filterA | filterB }}
{{ message | filterA('arg1', arg2) }}
```

