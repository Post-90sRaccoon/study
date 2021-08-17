# Vue.use()

## 使用

* 需要在调用 `new Vue()` 启动应用之前完成。

* 在 `Vue.use()` 执行时 install 会默认执行。

* 注册全局组件：

    ```javascript
    import a from './a'
    import b from './b'
    let components = { a, b }
    const installBase = {
      install (Vue) {
        Object.keys(components).map(key => Vue.component(key, components[key]))
      }
    }
    ```
    

## 使用场景

* 全局注入一个插件，不需要在每个文件中 import 插件。

## 内部原理摘要

```javascript
import { toArray } from '../util/index'

export function initUse (Vue: GlobalAPI) {
  Vue.use = function (plugin: Function | Object) {
    const installedPlugins = (this._installedPlugins || (this._installedPlugins = []))
    if (installedPlugins.indexOf(plugin) > -1) {
      return this
    }

    // additional parameters
    const args = toArray(arguments, 1)
    args.unshift(this)
    // 把除了第一个外的参数合成一个数组，把 vue 对象插到数组的第一位。
    if (typeof plugin.install === 'function') {
      plugin.install.apply(plugin, args)
    } else if (typeof plugin === 'function') {
      plugin.apply(null, args)
    }
    installedPlugins.push(plugin)
    return this
  }
}
```

1. 判断插件是对象还是函数：`Vue.use = function (plugin: Function | Object)`
2. 判断是否注册过这个插件。注册过 `return this`。`installedPlugins.indexOf(plugin) > -1`。
3. 判断插件是否有install方法，如果有就执行install()方法。没有就直接把plugin当Install执行。

## 功能

* 添加全局方法或者属性。如: `vue-custom-element`

* 添加全局资源：指令/过滤器/过渡/组件等。如 `vue-touch`

* 通过全局混入来添加一些组件选项。如 `vue-router`

* 添加 `Vue` 实例方法，通过把它们添加到 `Vue.prototype` 上实现。

```javascript
MyPlugin.install = function (Vue, options) {
  // 1. 添加全局方法或属性
  Vue.myGlobalMethod = function () {
    // 逻辑...
  }
  // 2. 添加全局资源
  Vue.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 逻辑...
    }
    ...
  })
  // 3. 注入组件选项
  Vue.mixin({
    created: function () {
      // 逻辑...
    }
    ...
  })
  // 4. 添加实例方法
  Vue.prototype.$myMethod = function (methodOptions) {
    // 逻辑...
  }
  // 5. 注册全局组件
  Vue.component('myButton',{
    // ...组件选项
  })
}
```

