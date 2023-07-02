# Object.create

## 实现类式继承

```javascript
// Shape——父类
function Shape() {
  this.x = 0;
  this.y = 0;
}

// 父类方法
Shape.prototype.move = function (x, y) {
  this.x += x;
  this.y += y;
  console.info("Shape moved.");
};

// Rectangle——子类
function Rectangle() {
  Shape.call(this); // 调用父类构造函数。
}

// 如果此时执行下面代码
const rect = new Rectangle();
console.log(rect);
//Rectangle { x: 0, y: 0 }
console.log(rect instanceof Shape);
// false
console.log(rect instanceof Rectangle);
// true
console.log(rect.move(1,1))
// rect.move is not a function

// 子类继承父类
Rectangle.prototype = Object.create(Shape.prototype, {
  // 如果不将 Rectangle.prototype.constructor 设置为 Rectangle，
  // 它将采用 Shape（父类）的 prototype.constructor。
  // 为避免这种情况，我们将 prototype.constructor 设置为 Rectangle（子类）。
  constructor: {
    value: Rectangle,
    enumerable: false,
    writable: true,
    configurable: true,
  },
});

console.log(rect instanceof Shape);
// true
console.log(rect instanceof Rectangle);
// true
console.log(rect.move(1,1))
// rect.move is not a function
```

