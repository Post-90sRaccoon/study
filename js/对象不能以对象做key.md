# JS 对象的 key

* 对象的 key 再使用非字符串时，js 将使用 toString() 方法进行类型转换。所以对象都会被转为 ‘[Obejct object]’。会被覆盖。
* Symbol 对象可以作为 key。
* 可以使用 map。

## 两种创建对象的比较

```javascript
var object=new Object();
object.2=1; //出错，因为变量不能以数字开头..

var object = {
  x: 1,
  2: 2
};
```

## 标识原则

1. 首字母必须是字母、下划线（_）或美元符号（$），不能是数字。

2. 除首字母外，其他字符可以是字母、数字、下划线或美元符号（$）。

3. 普通标识符（用作变量名、函数名和循环语句中用于跳转的标记）不能是保留字符或关键字。

4. 在严格模式下，arguments 和 eval 不能用作变量名，函数名或者参数名。

* 不符合标识符情况，key 必须加 “ ”，读取 key 对应的 value 只能是 obj[“key”]。



* es5 对象的 key 总是被解释为字符串。
* es6 允许使用计算的值作为对象的 key，使用 []。

```javascript
const color = 'red';
const product = {
  [`clothes${red}`] : true,
}
```

