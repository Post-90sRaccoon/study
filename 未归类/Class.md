# 类

[TOC]

## 定义类

类语法组成：类表达和类声明

### 类声明

```javascript
class Rectangle {
  //
}
```

* 函数声明会提升，类声明不会。

### 类表达式

```javascript
let Rectangle = class {
  //
}
```

* 类表达式可以命名可以不命名，命名是该类的局部名称，可通过类的 `name` 属性获得。

## 类体和方法定义

### 严格模式

类声明和类表达式的主体都执行在[严格模式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)下。

```javascript
class Animal {
  speak() {
    return this;
  }
  static eat() {
    return this;
  }
}

let obj = new Animal();
obj.speak(); // Animal {}
let speak = obj.speak;
speak(); // undefined

Animal.eat() // class Animal
let eat = Animal.eat;
eat(); // undefined
```

```javascript
function Animal() { }

Animal.prototype.speak = function() {
  return this;
}

Animal.eat = function() {
  return this;
}

let obj = new Animal();
let speak = obj.speak;
speak(); // global object

let eat = Animal.eat;
eat(); // global object
```

### 构造函数

一个构造函数可以使用 `spuer` 关键字调用一个父类的构造函数。

### 方法

```javascript
class Rectangle {
  // constructor
  constructor(height, width) {
      this.height = height;
      this.width = width;
  }
  // Getter
  get area() {
      return this.calcArea()
  }
  // Method
  calcArea() {
      return this.height * this.width;
  }
}
```

### 静态方法

`static` 关键字定义。调用静态方法不需要实例化类，不能通过类实例调用。常用于创建工具函数。

### 字段声明

#### 公有字段声明

```javascript
class Rectangle {
  height = 0;
  width;
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```

预先声明字段，自我记录，始终存在。声明不需要关键字。

#### 私有字段声明

```javascript
class Rectangle {
  #height = 0;
  #width;
  constructor(height, width) {
    this.#height = height;
    this.#width = width;
  }
}
```

私有字段仅能在字段声明中预先定义，不能通过之后赋值定义。

## 使用  extends 扩展子类

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name);
  }
  speak() {
    // 使用 super 调用父类的方法
    super.speak();
    console.log(this.name + ' roars.');
  }
}
```

如果子类中定义了构造函数，必须先调用 `super()` 才能使用 `this`。

可以使用 `extends` 继承基于函数的类。

不能用 `extends` 继承不可构造的对象。

```javascript
var Animal = {
  speak() {
    console.log(this.name + ' makes a noise.');
  }
};

class Dog {
  constructor(name) {
    this.name = name;
  }
}

Object.setPrototypeOf(Dog.prototype, Animal);
```

## Species

复制实例副本的方法时，覆盖构造函数。

```javascript
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}
var a = new MyArray(1,2,3);
var mapped = a.map(x => x * x);

console.log(mapped instanceof MyArray);
// false
console.log(mapped instanceof Array);
// true
```

## Mix-ins / 混入

```javascript
var calculatorMixin = Base => class extends Base {
  calc() { }
};

var randomizerMixin = Base => class extends Base {
  randomize() { }
};

class Foo { }
class Bar extends calculatorMixin(randomizerMixin(Foo)) { }
```

## 类私有域

类属性默认公有，增加`#` 前缀定义私有类段。

### 私有字段

私有字段包括私有实例字段和私有静态字段。

#### 私有实例字段

声明和访问都要加上 `#`。

```javascript
class Test {
  test = 1;
  constructor() {
    console.log(this.test);
  }
}
```

#### 私有静态字段

静态变量只能被静态方法调用。

```javascript
class ClassWithPrivateStaticField {
  static #PRIVATE_STATIC_FIELD;

  static publicStaticMethod() {
    ClassWithPrivateStaticField.#PRIVATE_STATIC_FIELD = 42;
    return ClassWithPrivateStaticField.#PRIVATE_STATIC_FIELD;
  }
}
```

只有定义该私有静态字段的类能访问该字段(子类不行)。

### 私有方法

#### 私有实例方法

```javascript
class ClassWithPrivateMethod {
  #privateMethod() {
    return 'hello world';
  }
}
```

#### 私有静态方法

只能在类声明内部访问它们。

```javascript
class ClassWithPrivateStaticMethod {
  static #privateStaticMethod() {
    return 42;
  }
}
```

