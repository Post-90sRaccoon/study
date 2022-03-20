### Vue 无法检测实例被创建时不存在 data 中的 property

### Vue 无法检测对象 property 的添加或删除

* 解决方案

```javascript
Vue.set(vm.obj, propertyName, newValue)
vm.$set(vm.obj, propertyName, newValue)

Vue.delete(vm.obj, propertyName)
vm.$delete(vm.obj, propertyName)
```

### Vue 不能检测通过数组索引直接修改一个数组项

* 解决方案

```javascript
Vue.set(vm.items, indexOfItem, newValue)
vm.$set(vm.items, indexOfItem, newValue)
vm.items.splice(indexOfItem, 1, newValue)
```

### Vue 不能检测直接修改数组长度的变化

* 使用 splice

### 在执行异步更新操作之前操作 DOM 数据不会变化

```javascript
vm.message = 'new message' // 更改数据
vm.$el.textContent === 'new message' // false
vm.$el.style.color = 'red' // 页面没有变化

vm.message = 'new message' // 更改数据
//使用 Vue.nextTick(callback) callback 将在 DOM 更新完成后被调用
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
  vm.$el.style.color = 'red' // 文字颜色变成红色
})
```

```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: {},
  }
})
vm.$nextTick(function () {
  this.message = {}
  this.message.text = '我更新啦！'
})

// text 属性没有声明，不是响应式，但是页面更新了。因为 DOM 更新是异步，setter 发生后，不会立刻更新，更新时 text 属性已经赋值
```

* 执行 `this.message = {};` 时， `setter` 被调用。
* Vue.js 追踪到 `message` 依赖的 `setter` 被调用后，会触发 `watcher` 重新计算。
* `this.message.text = '我更新啦！';` 对 `text` 属性进行赋值。
* 异步回调逻辑执行结束之后，就会导致它的关联指令更新 DOM，指令更新开始执行。

所以真正的触发模版更新的操作是 `this.message = {};`这一句引起的，因为触发了 `setter`，所以单看上述例子，具有响应式特性的数据只有 `message` 这一层，它的动态添加的属性是不具备的。

### 层级深，不更新

* `vm.$forceUpdate()`

### 路由参数变化，数据不更新

* 路由视图组件**引用了相同组件**时，当路由参会变化时，会导致该组件无法更新。
* 组件中通过 `watch` 监听 `$route` 的变化。
*  `<router-view>` 绑定 `key` 属性。如果跳到不是同一个组件，key 多余。