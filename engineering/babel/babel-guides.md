# Guides

## 什么是 Babel

### Babel 是 JavaScript 编译器

* 主要用于将 `ES 2015+` 的代码转换为向后兼容的代码。
    * 转换语法。
    * 填充环境缺失的特性。
    * 源码转换。

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
* 抽象成 AST 树。将插件转义好的内容生成 JS 代码。

```javascript
const babel = require("@babel/core");

babel.transformSync("code", optionsObject);
```

#### Cli tool

```javascript
npm install --save-dev @babel/core @babel/cli

./node_modules/.bin/babel src --out-dir lib
```

#### 插件和预设

* 预设是提前决定好的一系列插件的集合。

```javascript
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

