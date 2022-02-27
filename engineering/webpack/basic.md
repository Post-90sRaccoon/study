# Webpack

## 简介

* 静态模块打包器。

* 指定入口文件，根据依赖图，引入，生成 chunk。对 chunk 进行处理(打包)，打包好了输出 bundle 文件。

## 概念

### Entry

* 以哪个文件为入口打包，分析构建内部依赖图。

### Output

* 打包后的 bundles 输出位置及命名。

### Loader

* 处理非 JS 文件。

### Plugins

* 更强的能力。打包优化，压缩等。

### Mode

* development:

    > process.env.NODE_ENV 设为 development。

* production:

    > rocess.env.NODE_ENV 设为 production。

### 使用

* `npm i webpack webpack-cli -g`
* `npm i -D webpack webpack-cli`
* `webpack ./src/index.js -o ./bulid/built.js --mode=development`  开发环境 Entry 打包输出到 Output

