# ES Module and CommonJS

##  为什么会出现模块化

```html
<body>
  <script src="./a.js"></script>
  <script src="./b.js"></script>
  <script src="./c.js"></script>
</body>
```

### 全局污染

* 变量命名冲突。

### 依赖管理难以处理

* c.js 可以调用 b.js 和 a.js 的内容。反之不行。

## CommonJS

* 模块同步加载并执行模块文件，拷贝一份值出来。

### CommonJS 的使用

* 在 `commonjs` 中每一个 js 文件都是一个单独的模块，称为 module。
* module 中包含 CommonJS 规范的核心变量 exports、module.exports、require。
* exports 和 module.exports 将模块中的内容进行导出。
* require 导入其他模块（自定义模块、系统模块、第三方库模块）中的内容。

```javascript
// a.js
const name = 'a';
module.exports = function getName  (){
    return name
}
```

```javascript
// b.js
const getName = require('./a.js')
module.exports = function getPerson(){
    return {
        name:getName(),
        gender:'male'
    }
}
```

### CommonJS 实现原理

* 可以在 CommonJS 规范下每一个 js 模块上直接使用 `exports`, `modules`, `requrie` 三个变量。在 Node 环境下还存在 `_filename` , `_dirname`。
* 编辑过程中，CommonJs 对 js 的代码块进行了首尾包装。

```javascript
(function(exports,require,module,__filename,__dirname){
   const name = 'a';
    module.exports = function getName  (){
    return name
  }
})
```

*  CommonJS 规范下模块，会形成一个包装函数，模块代码将作为包装函数的执行上下文。

```javascript
function wrapper (script) {
    return '(function (exports, require, module, __filename, __dirname) {' + 
        script +
     '\n})'
}
```

```javascript
 runInThisContext(moduleFunction)(module.exports, require, module, __filename, __dirname)
```

* 模块加载时， `runInThisContext` (可以理解成 `eval` ) 执行 `moduleFunction`

### require 加载流程

```javascript
const fs = require('fs')      // 内置模块
const getName = require('./a.js')  // 文件模块
const _ =  require('lodash')   // 第三方自定义模块
```

* 优先找缓存=>内置模块=>文件模块=>自定义模块

![4.jpg](ES%20Module%20and%20Commonjs.assets/cfb7a91998774fc78a9813e3b0db8199tplv-k3u1fbpfcp-zoom-in-crop-mark1512000.webp)

### require 模块引入与处理

* CommonJS 模块同步加载并执行模块文件，在执行阶段分析模块依赖，**深度优先遍历**。

```javascript
//a.js
const getMessage = require('./b')
console.log('我是 a 文件')
exports.say = function(){
    const message = getMessage()
    console.log(message)
}
```

```javascript
// b.js
const say = require('./a')
const  object = {
   name:'Tim',
   gender:'male'
}
console.log('我是 b 文件')
module.exports = function(){
    return object
}
```

```javascript
// main.js
const a = require('./a')
const b = require('./b')

console.log('node 入口文件')

//我是 b 文件
// 我是 a 文件
// node 入口文件
```

* a,b 只执行了一次。
* 没有循环引用。
* 深度优先。

### require 加载原理

* module:  Node 中每一个 js 文件都是一个 module ，module 上保存了 exports 等信息之外，还有一个 **`loaded`** 表示该模块是否被加载。

```javascript
function require(id) {
   /* 查找  Module 上有没有已经加载的 js  对象*/
   const  cachedModule = Module._cache[id]
   
   /* 如果已经加载了那么直接取走缓存的 exports 对象  */
  if(cachedModule){
    return cachedModule.exports
  }
 
  /* 创建当前模块的 module  */
  const module = { exports: {} ,loaded: false , ...}

  /* 将 module 缓存到  Module 的缓存属性中，路径标识符作为 id */  
  Module._cache[id] = module
  /* 加载文件 */
  runInThisContext(wrapper('module.exports = "123"'))(module.exports, require, module, __filename, __dirname)
  /* 加载完成 *//
  module.loaded = true 
  /* 返回值 */
  return module.exports
}
```

* 解决了重复执行，循环引用的问题。

### require 动态加载

* require 可以在任意的上下文使用。

```javascript
const getMessage = require('./b')
exports.say = function(){
    const message = getMessage()
    console.log(message)
}

// 可以写为
exports.say = function(){
    const getMessage = require('./b')
    const message = getMessage()
    console.log(message)
}
```

### exports 和 module.exports

#### exports 使用

```javascript
// a.js
exports.name = 'Tim'
exports.gender = 'male'
exports.say = function (){
    console.log('哈哈')
}
```

```javascript
const a = require('./a')
console.log(a)
//{ name: 'Tim', gender: 'male', say: [Function] } 
```

* exports 是传入到当前模块的一个对象，传入的是 module.exports。exports 是形参，所以直接对 exports 赋值是无效的。

#### module.exports 使用

```javascript
module.exports ={
    name: 'Tim',
    gender: 'male',
    say(){
        console.log('哈哈')
    }
}
// 也可以导出单独的类和函数
module.exports = function (){
    // ...
}
```

* `exports` 和 `module.exports` 是相同的引用，所以一个文件选用一种即可，不然会覆盖。

* `module.exports` 如果导出非对象类型，循环引用时会丢失。对象类型存的地址，后绑定属性也可以通过异步方式访问到。

* [Node Module cjs 实现的源码](https://github.com/nodejs/node/tree/6686d9000b64a8cbfa2077ea0e30e253c35e053a/lib/internal/modules/cjs)

    

## ES Module

*  `ES6` 时， `JavaScript` 有了自己的模块化规范。
* `import` 和 `export`

### 具名导出

```javascript
const name = 'Tim' 
const gender = 'male'
export { name, gender }
export const say = function (){
    console.log('哈哈')
}
```

```javascript
import { name , gender , say } from './a.js'
// 名字要对应
```

### 默认导出

```javascript
const name = 'Tim' 
const gender = 'male'
const say = function (){
    console.log('哈哈')
}

export default {
    name,
    gender,
    say
} 
```

```javascript
import info from '.a/js'
// 名字不许对应
```

### 混合导出

```javascript
const name = 'Tim' 
const gender = 'male'

export default function say(){
    console.log('哈哈')
}
```

```javascript
import customizeName, { name, gender as sex } from './a.js'
import customizeName, * as info from './a.js'
```

### 重定向导出

```javascript
export * from './a.js'  // 只导出 不导入
```

### 不导入，只运行一次

```javascript
import './a.js'
```

### 特点

* 静态语法，`import` 自动提升到顶层，`import` 和 `export` 不能放在作用域条件语句中。提前加载并执行模块文件，确定依赖关系。
* 导入的变量只读。
* 被导入模块运行在严格模式下。
* 导入的变量是基本类型也可理解为引用传递。

```javascript
// a.js
export let num = 1
export const addNumber = ()=>{
    num++
}

// b.js 
import {  num , addNumber } from './a'

// 错误
num = 2

// 可以这样
console.log(num) // num = 1
addNumber()
console.log(num) // num = 2
```

