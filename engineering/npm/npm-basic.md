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
* windos 安装后 需要重启生效。

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

