## 杂项

## attribute 和 property 的不同

* [difference]:https://stackoverflow.com/questions/6003819/what-is-the-difference-between-properties-and-attributes-in-html#answer-6004028

    

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

```javascript
var obj ={
  foo: function () {
    console.log(this);
  }
};

obj.foo() // obj

// 情况一
(obj.foo = obj.foo)() // window
// 情况二
(false || obj.foo)() // window
// 情况三
(1, obj.foo)() // window

// obj.foo 就是一个值。这个值真正调用的时候，运行环境已经不是 obj 了，而是全局环境，所以 this 不再指向 obj。

// 上面三种情况等同于下面的代码。
// 情况一
(obj.foo = function () {
  console.log(this);
})()
// 等同于
(function () {
  console.log(this);
})()

// 情况二
(false || function () {
  console.log(this);
})()

// 情况三
(1, function () {
  console.log(this);
})()

// 如果this所在的方法不在对象的第一层，这时this只是指向当前一层的对象。
var a = {
  p: 'Hello',
  b: {
    m: function() {
      console.log(this.p);
    }
  }
};

a.b.m() // undefined
```

####  避免使用多层 this

```javascript
var o = {
  f1: function () {
    console.log(this);
    var f2 = function () {
      console.log(this);
    }();
  }
}

o.f1()
// object o
// window
// 解决方案 把外层 this 用一个变量记录下来

// 上面代码相当于
var temp = function () {
  console.log(this);
};

var o = {
  f1: function () {
    console.log(this);
    var f2 = temp();
  }
}
```

#### 避免数组处理方法中使用 this

```javascript
var o = {
  v: 'hello',
  p: [ 'a1', 'a2' ],
  f: function f() {
    this.p.forEach(function (item) {
      console.log(this.v + ' ' + item);
    });
  }
}
o.f()
// 原因同上 内层 this 不指向外层 指向顶层
```

#### 避免回调函数中的 this

```javascript
var o = new Object();
o.f = function () {
  console.log(this === o);
}

$('#button').on('click', o.f);
// 指向 DOM 对象 如果是箭头函数 指向 window
```

#### apply 的应用

```javascript
// 将数组空元素变为 undefined
Array.apply(null, ['a', ,'b'])
// [ 'a', undefined, 'b' ]
// forEach 跳过空元素，不会跳过 undefined
```

#### bind 的使用

* 事件监听函数不能直接 bind, 每次生成新函数，无法取消绑定。

* 使用 bind 将包含 this 的方法作为回调函数。

* 数组方法：

    ```javascript
    obj.print = function () {
      this.times.forEach(function (n) {
        console.log(this.name); //不 bind 这里指向全局对象
      }.bind(this));
    };
    
    obj.print()
    ```

* 与 call 方法结合使用：

    ```javascript
    [1, 2, 3].slice(0, 1)
    Array.prototype.slice.call([1, 2, 3], 0, 1)
    var slice = Function.prototype.call.bind(Array.prototype.slice);
    slice([1, 2, 3], 0, 1)
    
    function f() {
      console.log(this.v);
    }
    
    var o = { v: 123 };
    var bind = Function.prototype.call.bind(Function.prototype.bind);
    bind(f, o)() // 123
    ```

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

* **简写方法不能当构造函数使用**

    ```javascript
    const obj = {
      f() {
        this.foo = 'bar';
      },
    };
    // new obj.f(); 会报错
    ```
