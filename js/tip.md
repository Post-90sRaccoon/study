* 解构技巧

```javascript
const country = {
  province: {
    city: '北京',
    location: 'north',
  },
}

const { province: { city, location }, province } = country
```

* 数字分隔符

```javascript
const money = 1_000_000_000
```

* try catch finally

```javascript
function demo() {
  try {
    return 1
  } catch (err) {
    return 2
  } finally {
    return 3
  }
}
// 3
```

* 如果 finally 语句块中有返回值 那么这个值作为整个 try catch 语句的返回，无论 try catch 中是否有返回。

    finally 的运行时机是在 try 执行 return 之前。

```javascript
function demo() {
  try  {
    var a = 2;
    console.log('a1', a)
    return a;
  } catch(e) {

  } finally{
    console.log('a2',a)
    ++a
    console.log('a3',a)
  }
}
// 2
```

* 调试代码的方式 获取当前调用栈

```javascript
new Error().stack
```

* 生成随机字符串

```javascript
const str = Math.random().toString(36).substr(2, 10);
//36 进制包括了 a~z 0-9 0.xxxx 所有 substr 2
```

* 可选调用链

```javascript
a?.b?.c  
//不存在 ?前面直接短路返回 undefined
```

* 空白合并操作符

```javascript
result = a ? a : b
result = a || b

//如果 a 是 0 undefined '' 且是有效值会被覆盖。

let c = a ?? b; 
//如果左侧返回 undefined 或者 null, 就返回右侧的 b
```

* promise.allSettled

* String.prototype.matchAll

* Dynamic import

    

    在事件回调中动态加载

    

    import(module) 可以在任何地方调用，返回一个模块对象的 promise,支持 await。

* BigInt

* globalThis 获取 this 的统一方法 多种环境。