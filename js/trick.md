# Trick

## 数据类型判断

```javascript
Object.prototype.toString.call(obj).slice(8, -1)
```

## 继承

### 原型链继承 (属性和方法)

```javascript
function Animal() {
    this.colors = ['black', 'white']
}
Animal.prototype.getColor = function() {
    return this.colors
}
function Dog() {}
Dog.prototype =  new Animal()

let dog1 = new Dog()
dog1.colors.push('brown')
let dog2 = new Dog()
console.log(dog2.colors)  // ['black', 'white', 'brown']
```

* 问题1：原型中包含的引用类型属性将被所有实例共享。
* 问题2：子类在实例化的时候不能给父类构造函数传参。

### 构造函数继承（属性和方法）

```javascript
function Animal(name) {
    this.name = name
    this.getName = function() {
        return this.name
    }
}
function Dog(name) {
    Animal.call(this, name)
}
```

* 解决上面了两个问题，但是方法在构造函数中，每次定义都会调用。