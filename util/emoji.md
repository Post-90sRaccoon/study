# Emoji

## ASCLL

## ASCLL 码是什么

基于拉丁字母的电脑编码系统，主要用于显示现代英语。

## Unicode

### Unicode 是什么

`Unicode` 是一种字符编码标准，它为世界上几乎所有的文字系统中的每个字符都分配了一个唯一的数字标识。这些标识被称为 `Unicode 码点`，通常用 `U+`（Unicode 转义符号）后跟着一个或多个十六进制数字来表示。

### Unicode 平面

`Unicode` 首先承认了 `ASCII` 占用 **0-127**（8位）。然后占用 **128-65535**（16位），又把之后的 **16 个 65536** 的数字即 **65536-1114111** （10000 1111111111111111 21位，其中16个1）的整数资源给占了，然后把多占的 **16 个 65536** 的段分别命名为 **16 个平面**，加上原来的 **0-65535 平面**，`Unicode` 总共有 **17 个平面**。

**第0平面（Plane 0）**，从 `U+0000`至`U+FFFF`，其中的字符最常用，**65535** 之后字符大多数是 `emoji` 表情。

### 实现方式

`Unicode` 的实现方式称为 `Unicode` 转换格式（Unicode Transformation Format，简称为 UTF）。其中最流行的是`UTF-8`、`UTF-16`、`UCS4/UTF-32`等。

#### UTF-8

`UTF-8` 使用 1 到 4 个字节来表示一个 `Unicode` 码点。

#### UTF-16

`JS` 语言采用的是Unicode字符集，`utf-16` 的编码方式。

`UTF-16` 采用 2 个或 4 个字节表示一个字符。

在辅助平面（码位范围U+10000-U+10FFFF）内的码位在`UTF-16`中被编码为一对`16bit`的码元（即32bit,4字节），称作**代理对(surrogate pair)**。组成代理对的两个码元前一个称为 **前导代理(lead surrogates)** 范围为`0xD800-0xDBFF`，后一个称为 **后尾代理(trail surrogates)** 范围为`0xDC00-0xDFFF`。

🐳 -> **\uD83D\uDC33**

🐳 -> **\uD83D\uDC33**

🐠 -> **\uD83D\uDC20**

#### UTF-32

`UTF-32` 使用 4 个字节表示一个 `Unicode` 码点。

#### emoji 字符

除了单个的emoji字符，还可以使用U+200D零宽连字(ZWJ)将两个emoji连起来，使其看起来像是一个emoji（不支持的系统会忽略零宽连字）。

U+1F468男人、U+200D ZWJ、U+1F469女人、U+200D ZWJ、U+1F467女孩（👨‍👩‍👧）在系统支持的情况下会显示为一个男人一个女人和一个女孩组成的家庭emoji，而不支持的系统则会顺序显示这三个emoji（👨👩👧）



## JavaScript 处理 emoji

### 长度

使用 `[...string].length` 或 `Array.from(string)` 如果组合 emoji 会分开，长度会不正确。

### 码点与字符的转换

`es6` 方法

`s.codePointAt()`

`String.fromCodePoint()`

### 字符串遍历

可使用 `for of` 遍历，但复合 `emoji` 仍然会分开。

[[同一个字符的两种 UTF-8 编码](https://www.cnblogs.com/EQ1024/p/16551168.html)](https://www.cnblogs.com/EQ1024/p/16551168.html)

## web中实现表情符号输入

[web 实现表情符号输入](https://acgtofe.com/posts/2021/03/web-emoji-input)

## 实现方式

### 自定义emoji 表情

使用图片。

### 系统默认表情