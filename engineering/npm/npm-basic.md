## 基本概念

* registry: package 的资料来源库。

### 登录

* `npm login` 官方源时才能登录。
* `npm whoani` 查看登录用户

### 下载最新版本

```bash
npm install npm@latest -g
```

### 下载 Node.js 和 npm

#### 使用 Node 版本管理工具下载 Node.js 和 npm

* 下载并切换多个版本
* OSX or Linux  
    * nvm
    * n
* windows 
    * nodist
    * nvm-windows
* 下载前卸载原来的 Node.js，使用 `npm root -g` 查看全局命令安装位置。
* windows 安装后 需要重启生效。

#### nvm

* 输入 nvm 查看用法。
* 常用 `nvm list`,`nvm-install`,`nvm-use`
* 下载失败，`nvm root` 有中文字符，手动设置一遍路径即可。
* nvm use 需要管理员运行。

## 问题

### debug.log

* 当包下载或发布失败，用一下命令将产生一个 npm-debug.log 的文件。

```bash
npm install --timing
npm publish --timing
```

* 在 .npm 文件夹中。

```bash
npm config get cache
```

* 使用 CI 环境，logs 会在其他位置。如 /build

### 常见问题

#### 奇怪问题

* npm cache clean
* npm install –verbose
* 详见文档。

## Packages and modules

### package

* 一个发布在 npm registry 包含 `package.json` 的文件。

#### scoped

* 登录后根据你的用户名或组织名获取一个 scope。可以用作命名空间。
* 形如 `@tim/package-name`。
* unscoped 都是 public，scoped 默认是 private。

#### 格式

* 需要 `package.json`。
* 详见文档。

### modules

* 在 `node_modules` 中能被 Node.js `require()` 加载
* 能被 `requrie()` 加载需要满足下面一个条件：
    * 一个文件夹有 `package.json` 里面有 `main` 字段。
    * 单纯的 JS 文件。

### 比较异同

* modules 不一定是 package。

### Package.json

* 列出所有依赖。
* 指定版本。

#### fields

* 必须有 `name` 和 `version`。
* `name` 必须小写，一个词，可以有连字符和下划线。
* `version` 必须是 x.x.x 的形式。

#### 创建

* 使用 `npm init`。

* 可以自定义问题 和 `fields`。创建 `.npm-init.js`

    * 自定义问题 `module.exports = prompt("what's xxx ?", "I LIKE THEM ALL");`

    * 自定义 fields

        ```javascript
        module.exports = {
          customField: 'Example custom field',
          otherCustomField: 'This example field is really cool'
        }
        ```

### 创建 Node.js modules

#### 创建 package.json

* 如果使用 npmrc 管理多个账户，切换 `npmrc <profile-name>`

* 如果使用 git 管理 package `git init git remote add origin git://git-remote-url`

* `npm init --scope=@scope-name`
* package 的 name 需要 unique
* ``npm init``

#### 创建 require 后加载的文件

```javascript
exports.functiona = function() { console.log('function a') };
```

#### 发布

* 发布 scoped 公开包 `npm publish --access public`
* 其他 `npm publish`

### 创建 README

* 更新发布 README `npm version patch  npm publish`

### 指定依赖

* `dependencies`  生产环境需要的依赖
* `devDependencies` 仅仅本地环境需要的
