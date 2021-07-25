## 杂项

### isNaN

* 判断一个数 Is Not A Number。

### Number.isNaN

* 判断一个数是不是 NaN。

### 对象的比较

* 对于两个对象，大于小于运算符比较的是值。等于是地址。

### 数组方法

```javascript
console.log([undefined, null].join('*'));
// *

const arr = ['a', 'b', 'c'];
console.log(arr.slice(2, 1));
// []
```

* `slice` 的应用：通过 `call` 将类数组转化为数组。
* `map` 方法的第二个参数用来绑定 `this`。

### JSON 方法

`JSON.stringify()`:

* 对象属性是 `undefined`、函数会被过滤。
* 数组成员是 `undefined`、函数会被转换成 `null`
* 会忽略不可枚举属性。

### this 相关

* 严格模式下，this 指向顶层对象，就会报错。

### 解构赋值

* 对象的解构,变量必须与属性同名。

* 解构赋值的应用：交换变量的值。

* 解构赋值和默认赋值的联合使用。

    ```javascript
    function foo({ x, y = 5 }) {
      console.log(x, y);
    }
    foo();
    // 只用了解构赋值的默认值，没有使用函数的默认值，所以 foo() 会报错。
    
    function bar({ x, y = 5 } = {}) {
      console.log(x, y);
    }
    bar();
    // undefined 5
    ```

* 设置的默认参数有单独作用域。

    ```javascript
    var x = 1;
    function foo(x, y = function() { x = 2; }) {
      var x = 3;
      y();
      console.log(x);
    }
    
    foo();  // 打印 3
    x       // 输出 1
    
    var x = 1;
    function foo(x, y = function() { x = 2; }) {
      x = 3;
      y();
      console.log(x);
    }
    
    foo();  // 打印 2
    x       // 输出 1
    ```

* 默认参数值的应用，不写某个参数报错：

    ```javascript
    function throwIfMissing() {
      throw new Error('不能省略');
    }
    
    function fob(val = throwIfMissing()) {
      return val;
    }
    
    fob();
    ```

    

### 函数与类

* 构造函数缺点： 同一个构造函数的多个实例之间，无法共享属性，比如函数属性，每个实例会创建一个新的方法。
* 使用 prototype, 所有实例都能共享。
