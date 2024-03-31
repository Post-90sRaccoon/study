# Guides

## 什么是 Babel

### Babel 是 JavaScript 编译器

* 主要用于将 `ES 2015+` 的代码转换为向后兼容的代码。
    * 转换语法。
    * 填充环境缺失的特性。
    * 源码转换（codemods[^codemods]）。
    
    [^codemods]:"codemods" 是一个缩写，代表的是 "code modifications"（代码修改）的意思。在编程领域，"codemods" 通常指的是一种自动化重构代码的工具或脚本。这些工具允许开发者通过定义特定的代码转换规则，对代码库中的大规模代码进行自动化修改，而无需手动逐行修改。

### 编译器

Parsing, Transformation and Code Generation



解析（Parsing）通常被分为两个阶段：词法分析（Lexical Analysis）和语法分析（Syntactic Analysis）。

1. **词法分析（Lexical Analysis）：** 在编译器或解释器中，词法分析是解析的第一个阶段。它负责将源代码（或其他程序输入）分割成一个个称为“词法单元”或“令牌(token)”的小块。这些词法单元可以是关键字、标识符、操作符、常量等。词法分析器（Lexer）扫描源代码，根据编程语言的规则，将代码分解成词法单元的序列。这个阶段的目标是去除空格、注释等不必要的字符，提取出程序中实际有意义的单词。

2. **语法分析（Syntactic Analysis）：** 一旦源代码被分割成词法单元，接下来的阶段是语法分析。在这个阶段，解析器（Parser）将词法单元的序列转化为抽象语法树（Abstract Syntax Tree，AST）。抽象语法树是一种树状结构，表示了程序的语法结构，其中每个节点代表一个语法结构，例如表达式、语句、函数等。语法分析器根据编程语言的语法规则，确保代码是否符合语言的语法要求，并将代码的结构以树状的形式表示出来，以便后续的编译或解释过程使用。

    

**转换（Transformation）：** 转换阶段是指对抽象语法树进行修改、优化或扩展的过程。在这个阶段，编译器可以对抽象语法树进行各种操作，例如优化、重构、检查错误、添加注解等。转换的目的是改善代码的质量、性能或其他方面的特性。优化编译器会在这个阶段应用各种优化技术，例如常量折叠、死代码删除、循环展开等，以提高程序的执行效率。



**代码生成（Code Generation）：** 代码生成阶段是编译器的最后一个阶段，它将经过解析和转换的源代码（或抽象语法树）翻译为目标代码。目标代码可以是汇编语言、机器语言或其他中间代码形式，具体取决于编译器的设计和目标平台。代码生成的目标是生成与源代码等价的、可在目标平台上运行的代码。

## Pluggable

* babel 由插件组成。
* `preset` 一系列的插件组成。

## 使用手册

### 概述

```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

* 根目录创建 `babel.config.json`

* 编译 `./node_modules/.bin/babel src --out-dir lib` 或 `npx babel src --out-dir lib`

### CLI 的基本用法

#### Core Library

* Babel 的核心功能在 `@babel/core` 中。 
* 抽象成 AST 树，方便各个插件分析语法进行相应的处理，将插件转义好的内容生成 JS 代码。

```javascript
const babel = require("@babel/core");

babel.transformSync("code", optionsObject);
```

#### Plugins & Presets

* 预设是提前决定好的一系列插件的集合。

```bash
npm install --save-dev @babel/preset-env

./node_modules/.bin/babel src --out-dir lib --presets=@babel/env
```

* 先执行插件，在执行预设。插件按声明顺序执行，预设逆序执行。

#### 配置

* 使用 `babel.config.json`

#### Polyfill

```javascript
import "core-js/stable";
import "regenerator-runtime/runtime";
```

* 如果使用 `env`  preset ,设置`useBuiltIns`为 `usage`, babel 会检查你的代码，只添加你环境缺失的。如果不使用这个 preset，必须在入口的最前面引入完整的 polyfill。

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1"
        },
        "useBuiltIns": "usage"
      }
    ]
  ]
}
```

#### 配置 Babel

* `babel.config.json` ：使用 monorepo (一个项目多个子包，多个 package.json，自爆间存在相互依赖)或想要编译 `node_modules`。
* `.babelrc.json`： 编译某个文件或某目录的子集。别名`.babelrc`。[详情](https://zhuanlan.zhihu.com/p/367724302)
* 也可以在 `package.json` 中的  `babel` 字段中配置。
* 可以使用  JS 写配置文件，可以使用 Node.js APIS。

#### 打印生效配置

```shell
BABEL_SHOW_CONFIG_FOR=${PATH} npm start
```

* 升序优先级打印。

#### 配置项合并

* 一般不是 `undefined` 会直接替换。

* `assumptions` , `parserOpts`,`generatorOpts` 是合并不是替换。

* 对于 `plugins` 和 `presets` 

    ```javascript
    plugins: [
      './other',
      ['./plug', { thing: true, field1: true }]
    ],
    overrides: [{
      plugins: [
        ['./plug', { thing: false, field2: true }],
      ]
    }]
    // merge 
    plugins: [
      './other',
      ['./plug', { thing: false, field2: true }],
    ],
    ```

* 对于两个同样插件，如果不想后面的覆盖前面的，需要给两个不同的名字。

```javascript
plugins: [
  ["./plug", { one: true }, "first-instance-name"],
  ["./plug", { two: true }, "second-instance-name"],
];
```

