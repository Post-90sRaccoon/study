# Basic

## 创建一个应用

### 应用实例

```javascript
import { createApp } from 'vue'

const app = createApp({
  /* 根组件选项 */
})
```

应用实例上有很多全局方法，如 `component` 方法。

实例上的方法大多返回实例。

### 根组件

```javascript
import { createApp } from 'vue'
// 从一个单文件组件中导入根组件
import App from './App.vue'

const app = createApp(App)
```

### 挂载应用

```javascript
app.mount('#app')
```

应用根组件的内容将会被渲染在容器元素里面。容器元素自己将**不会**被视为应用的一部分。

`.mount()` 方法应该始终在整个应用配置和资源注册完成后被调用。同时请注意，不同于其他资源注册方法，它的返回值是根组件实例而非应用实例。

## 模板语法

### 调用函数

绑定在表达式中的方法在组件每次更新时都会被重新调用，因此**不**应该产生任何副作用，比如改变数据或触发异步操作。