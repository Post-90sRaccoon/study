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

```bash
npm install --save-prod
npm install --save-dev
```

### 语义控制版本

* 第一次发布 `1.0.0`
* 向后兼容的 bug 修复 `1.0.1`
* 向后兼容的新特性 `1.1.0`
* 破坏向后兼容的修改 `2.0.0`

#### 规则符号

* `^` ：只会执行不更改最左边非零数字的更新。
* `~` :  只会执行补丁版本。~0.13.0 可以更新到 0.13.1，不能更新的 0.14.0。
* `>`: 接收高于指定版本的任何版本。``>=` 高于等于
* `<` : 低于 `<=` 低于等于  `=` 等于
* `-`:  2.1.0 - 2.6.2 范围
* `||`: 组合。`1.0.0 || >=1.1.0 <1.2.0`，即使用 1.0.0 或从 1.1.0 开始但低于 1.2.0 的版本。
* 无符号任意版本，`latest` 使用最新版本。

### 加 tag 来区分版本

```bash
npm publish --tag <tag>
npm dist-tag add <package-name>@<version> [<tag>]
```

### 下载包

```bash
npm install <package_name> 
npm install @scope/package-name
npm install <package_name>@<tag>
```

* 如果没有 `node_modules` 会创建并在里面下载。
* 如果没有 `package.json` 会下载最新版本。
* 全局下载加 `-g`

### 更新包

```bash
npm update // 更新所有，也可以指定包
npm outdated // test 
npm install npm@latest -g  // 全局的包更新
npm outdated -g --depth=0 // 查看全局哪些包需要更新
```

### 卸载包

```bash
npm uninstall <package_name>  // 从 node_modules 删除
npm uninstall --save lodash // 从 package.json 删除
// 全局卸载加 -g
```

### 命令行

#### npm

* 如果下载 `native code` module 需要编译 `C++`，需要使用 `node-gyp`。Unix 需要 `Python`, Windows 需要`Python` 和 `Microsoft Visual Studio C++`。

#### npm audit

* `npm audit` : 分析复杂的代码，列出项目依赖中有安全漏洞的版本。
* `npm audit fix`: 检测项目依赖中的漏洞并自动更新有漏洞的依赖。

#### npm cache

* `npm cache add`

* `npm cache clean --force`

    * 配置文件中的 `cache` 字段配置的是根目录

    * 缓存数据放在根目录中的 `_cacache` 文件夹中

    * `clean` 命令清除的是 `_cacache` 文件夹

    * `~/.npm/_cacache` 中存的是一些二进制文件，以及对应的索引。

        npm install 时，有缓存的话，会通过 pacote 把对应的二进制文件解压到相应的 `node_modules` 下面。

        npm本身只提供清除缓存和验证缓存完整性的方法，不提供直接操作缓存的方法，可以通过 `cacache` 来操作这些缓存数据。

* `npm cache verify` 验证缓存完整性。

#### npm ci

* `npm ci` 类似于 `npm-install` ，但它旨在用于自动化环境，如测试平台，持续集成和部署。
* 必须有一个 `package-lock.json` 文件。
* `package-lock.json` 在执行 npm i 的时候生成，用来记录实际安装的 npm 包的来源和版本。可以锁定安装时的包的版本，需要上传到 git，确保大家使用的包版本一致。
* 安装前自动清除现存的 `node_moduels`。

#### npm outdated

* 列出所有没有升级到最新版本的项目依赖。红色可以无脑升级，`npm update` 会一次性升级所有红色依赖。

#### npm repo

* 打开项目的源代码仓库。`npm honme` 打开项目主页。

#### npm run

* `package.json` 的 `script` 中的命令，如`“foo”: "echo foo"`可以通过 npm run foo 运行对应命令。与 `make` 类似。直接 `make` 执行默认任务，直接运行 `run`, 会列出所有命令。

#### npx

* 为了确保开发环境的一致性。`npm -i -g` 应该慎用。日常工具可以使用，项目开发所需的工具，作为开发依赖安装，使用 `npx` 调用。

    ```bas
    npm i -D webpack
    npx webpack
    ```

* 一次性临时任务，可以直接调用，不会污染 devDependencies

    ```bash
    npx create-react-app test-app
    路径中如果找不到 create-react-app，会自动安装。临时安装，项目初始化完后就会删除。
    ```

    * 会先找当前项目的node_modules/.bin里面的create-react-app，找不到就找全局的create-react-app，再找不到就找npm缓存，还找不到就找npm仓库的。
    * 如果是找的npm仓库的，那就会下载回来，放到缓存里面。

* 指定 node 版本 来运行 npm script 用来测试不同版本兼容性。

  ```bash
  npx -p node@12 npm run bulid
  -p 指定包名
  ```

* 自动查找当前依赖包中的可执行文件。到 `node_modules/.bin` 和环境变量 `$PATH`（执行命令先到环境变量找，找不到当前目录找） 里，如果依然找不到，就会帮你安装。所以可以调用系统命令如 `ls`，cd 是 `bash` 命令，不能用 npx 调用。

    ```bash
    npx webpack -v
    ```

    * 执行远程仓库的可执行文件

        ```bash
        npx github:piuccio/cowsay hello
        ```

        

* `--no-install` 使用本地模块。 `--ignore-existing` 使用远程模块。

* 交互式开发  `npm run-script`

    * `npx -p cowsay -p lolcatjs -c 'echo "$ npm_package_name@$npm_package_version" | Cowsay | lolcatjs'` 允许脚本从运行脚本访问一系列$ npm_`变量。

    *  npx 安装多个模块，有第一个可执行项会使用 npx 安装的模块。后面的可执行项还是会交给 Shell 解释。

        ```bash
        npx -p lolcatjs -p cowsay -c 'cowsay hello | lolcatjs'
        ```

    * 将环境变量带入所执行的命令 `npm run env | grep npm_`。

    * 执行npm run会新建一个shell，并在这个shell里面执行指定的脚本命令，并且会将当前目录的node_modules/.bin 目录加入PATH（环境变量）里面，执行结束后，再将PATH恢复原样
    
      
      
        
