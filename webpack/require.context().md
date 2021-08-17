# Require.context()

## 作用

* 获取特定上下文，自动化导入模块。

## 使用

* 接收三个参数。
    * `directory{string}`：读取文件路径。
    * `useSubdirectories {Boolean}` ：是否遍历文件的子目录。
    * `regExp {RegExp}`： 匹配文件的正则。 

* 返回一个函数，这个函数有 3 个属性。
    * `resolve {Function}` ，接收一个参数 request，是 directory 文件下匹配的相对路径，返回这个匹配文件相对于整个工程的相对路径。
    * `keys {Function}` : 返回匹配成功模块的名字组成的数组。
    * `id {String}`: 执行环境的 id。id属性返回了匹配的文件夹的相对于工程的相对路径,是否遍历子目录,匹配正则组成的字符串。
    * 返回函数本身接收一个 request 参数，返回一个模块，和 `import` 一样。

* ```javascript
    require.context`可以理解为内部将所有模块整合为一个`map`，模块以路径为`key
    
    {
    './a.js':model,
    './b/f.js':model,
    ...
    }
    
    require.context返回一个函数，接受一个key参数,然后返回对应key的模块
    require.context返回的函数有三个属性:
    1.resolve 是一个函数，它返回 request 被解析后得到的模块 id。
    2.keys 是一个函数，返回一个有所有模块的key组成的数组
    3.id 是 context module 里面所包含的模块 id
    ```