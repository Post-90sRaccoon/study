[Object.defineProperty]:https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty
[Object.defineProperties]: https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties

```javascript
obj = Object.freeze({ a: 1, b: 2 })
//不能增加 不能删除 修改没用

Object.seal({ a: 1, b: 2 })
//seal 可以修改属性 不能删除 不能增加

Object.preventExtensions({ a: 1, b: 2 })
//阻止增加属性 可以删除属性
```

