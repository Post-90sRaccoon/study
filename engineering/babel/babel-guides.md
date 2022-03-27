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

