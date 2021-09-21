# 微信小程序

* https://mp.weixin.qq.com/

## 小程序代码构成

### `app`.json

* `app.json `是当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。

  ```json
  {
  	"pages": [],
  	"window": [],
  }
  ```

  

* `entryPagePath` 小程序默认启动首页
  `tabBar` 底部 tab 栏
  `networkTimeout` 网络超时时间
  `debug` 是否开启 debug 模式

### project.config.json

* 开发者工具的配置

### page.json

* `enablePullDownRefresh` 是否开启当前页面下拉刷新
  `onReachBottomDistance` 页面上拉触底事件触发时距页面底部距离 单位是 px
  `disableScroll` 页面不能上下滚动
  `usingComponents` 页面自定义组件配置
  `initialRenderingCache` 页面初始渲染缓存配置

## 小程序的宿主环境

### 渲染层和逻辑层

* 由2个线程管理：渲染层使用了 `WebView` 进行渲染；逻辑层采用 `JsCore` 线程运行 `JS` 脚本。一个小程序存在多个界面，所以渲染层存在多个 `WebView` 线程，这两个线程的通信由微信客户端（ `Native` 代指微信客户端）做中转，逻辑层发送网络请求也经由 `Native` 转发。

![img](https://res.wx.qq.com/wxdoc/dist/assets/img/4-1.ad156d1c.png)

### 程序与页面

* 客户端打开小程序前，把小程序代码包下载到本地。
* 通过 `app.json` 的 `pages` 字段获得当前小程序的所有页面路径。`pages` 字段第一个页面就是小程序的首页。

* 小程序启动后 在 `app.js` 中的：

  ```javascript
  App({
    onLaunch: function() {
  
    }
  })
  ```

  回调会执行。全部页面共享这个 `APP` 实例。

* 页面先根据 `JSON` 生成一个界面，然后装载 `WXML` 和 `WXSS ` 样式。最后客户端装载 `JS`。生成页面时，小程序框架会把 `data` 数据和 `index.wxml` 一起渲染出最终的结构。渲染后触发一个 `onLoad` 的回调。可以在这个回调函数处理逻辑。
* 通过 `setData()` 从逻辑层往渲染层发送数据。

## 小程序框架

### 获取场景值

* 可以在 `App` 的 `onLaunch` 和 `onShow`，或 `wx.getLaunchOptionsSync` 取到。

### 逻辑层 `App` Service

* https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
* 在 `JS` 基础之上 增加
      `App` 和 `Page` 方法，进行程序注册和页面注册。
      `getApp` 和 `getCurrentPages` 方法，获取 `App` 实例和当前页面栈。
      每个页面有独立作用域。

#### 注册小程序 `app.js`

```javascript
App({
  //onLaunch(options) 初始化完成时触发，全局只触发一次。参数可用 wx.getLaunchOptionsSync 获取。 
  //onShow 启动或后台切入前台触发
  //onHide onError onPageNotFound onUnhandledRejection onThemeChange
})

```

* 整个小程序只有一个 `App` 实例，全部页面共享的。开发者可以通过 `getApp` 方法获取到全局唯一的 `App` 实例，获取 `App` 上的函数。

#### 注册页面 `page.js`

* `data`  页面第一次渲染使用的初始数据 渲染层可以通过 `WXML` 数据进行绑定。

  ```html
  <view>{{text}}</view>
  ```

* `options` 页面的组件选项。

* `onLoad(options)`  页面加载时触发 一个页面只会调用一次，可以在 `onLoad` 的参数中获取打开当前页面路径中的参数，通过 `query` 获得。

* `onReady` 页面初次渲染完成 一个页面只会调用一次，代表页面已经准备完成，可以和视图层进行交互
  对页面内容进行设置的 `API` 应在 `onReady` 后使用。

* `onUnload` 生命周期回调监听页面卸载。

* `onPullDownRefresh` 用户下拉， `onPageScroll` 页面滚动， `onReachBottom` 页面上拉触底事件的处理函数。

* 用户分享相关方法。

* `onSaveExitState` 页面销毁前保留状态回调。

* `onTabItemTap` 点击 `tab`。

* `onResize`

* 组件事件处理函数：

  ```html
  <view bindtap="viewTap"> click me </view>
  
  <script>
  viewTap: function() {
    this.setData({
      text: 'Set some data for updating view.'
    }, function() {
      // this is setData callback
    })
  }
  </script>
  ```

* `Page.route` 到当前页面的路径 类型为 `String`。
* `Page.prototype.setData(Object data, Function callback)`
  * 将数据从逻辑层发送到视图层(异步) 同时改变对应的 `this.data` 的值 同步。
  * `Object` 以 `key: value` 的形式表示  将 `this.data` 中的 `key` 对应的值改变成 `value`。
  * `key` 可以以数据路径的形式给出 如 `array[2].message`,`a.b.c.d`,比修改整个对象或数组要好，并且不需要在 `this.data` 中预先定义。

* 必须通过 `this.setData` 而不能直接修改 `this.data`。或者修改 `this.data` 后马上用 `setData` 设置一下
2. 仅支持设置可 `JSON` 化的数据, 不能设置为 `undefined`。
3. 单次设置的数据不能超过1024 `kB`。

#### 页面间通信

* 如果一个页面由另一个页面通过 `wx.navigateTo` 打开， 这两个页面间将建立一条数据通道。
* 被打开的页面可通过 `this.getOpenerEventChanel()` 方法获得一个 `EventChanel` 对象。
* `wx.navigateTo` 的 `success` 回调中也包含一个 `EventChanel` 对象
* 这两个 `EventChanel` 对象间可 通过 `emit` 和 `on` 方法相互发送，监听事件。

```javascript
// opener
Page({
  jump: function () {
    wx.navigateTo({
      url: './test',
      events: {
        acceptDataFromOpenedPage: function (data) {
          console.log(data)
        },
      },
      success: function (res) {
        res.eventChannel.emit('acceptDataFromOpenerPage', { data: 'send from opener page' 		})
      }
    })
  },
})

//opened
Page({
  onLoad: function (option) {
    const eventChannel = this.getOpenerEventChannel()
    eventChannel.emit('acceptDataFromOpenedPage', { data: 'send from opened page' });
    eventChannel.on('acceptDataFromOpenerPage', function (data) {
      console.log(data)
    })
  }
})

// console
{data: "send from opened page"}
{data: "send from opener page"}
```

* 页面可以引用 `behaviors` 。 `behaviors` 可以用来让多个页面有相同的数据字段和方法。
* `Page` 构造器适用于简单的页面。但对于复杂的页面,可以使用 `Component` 构造器来构造页面。

#### 小程序的生命周期

![img](https://res.wx.qq.com/wxdoc/dist/assets/img/page-lifecycle.2e646c86.png)

#### 页面路由

* 小程序的页面栈最多存储是个页面，栈满之后 `wx.navigateTo` 将会失败。跳转逻辑可能成环时，使用其他路由 `API` 避免爆栈。 

##### 页面栈

![image-20210915160657864](C:\Users\maiscrm\AppData\Roaming\Typora\typora-user-images\image-20210915160657864.png)

##### 路由方式

![image-20210915160955194](C:\Users\maiscrm\AppData\Roaming\Typora\typora-user-images\image-20210915160955194.png)

![image-20210915161451117](C:\Users\maiscrm\AppData\Roaming\Typora\typora-user-images\image-20210915161451117.png)

* `navigateTo`, `redirectTo` 只能打开非 tabBar 页面。
* `switchTab` 只能打开 tabBar 页面。
* `reLaunch` 可以打开任意页面。
* 页面底部的 tabBar 由页面决定，即只要是定义为 tabBar 的页面，底部都有 tabBar。
* 调用页面路由带的参数可以在目标页面的`onLoad`中获取。

#### `API`

##### 事件监听 `API`

* 以 `on` 开头的 `API` 用来监听某个事件是否触发。接受一个回调函数作为参数，当事件触发时会调用这个回调函数。并将相关数据以参数形式传入。

##### 同步 `API`

* 以 `Sync` 结尾的 `API` 都是同步 `API`。执行结果直接获取，执行出错会抛出异常。

##### 异步 `API`

* 接收一个 `Object` 类型的参数。支持 `success` `fail` `comolete`(失败成功都会执行)。结果通过回调函数的参数获取。
* 接口参数不包含 `success/fail/complete` 时默认返回 `promise`。

##### 跳转

* wx.navigateToMiniProgram
* wx.navigateBackMiniProgram
* wx.exitMiniProgram

##### 交互

* wx.showToast  显示消息提示框。
* wx.hideToast 隐藏消息提示框。
* wx.showModal 显示模态对话框。
* wx.showLoading 显示loading 提示框，需要主动调用 wx.hideLoading 才能关闭提示框。
* wx.showActionSheet 显示操作菜单。
* wx.enableAlertBeforeUnload 开启小程序返回询问页面对话框。
* wx.disableAlertBeforeUnload 关闭小程序页面返回询问对话框

##### 下拉刷新

* wx.stopPullDownRefresh 支持 Promise 风格调用。停止当前页面下拉刷新。
* wx.startPullDownRefresh 调用后触发下拉刷新动画，效果与用户手动刷新一致。
* 需要在 `page.json` 配置 `enablePullDownRefresh` 才可以触发下拉刷新。
* 需要在 `page.json` 配置 `backgroundTextStyle` 才可以显示下拉动画。因为背景色和点点色一致。

##### 位置

* 获取位置信息需要配置地理位置用途说明：

  ```json
  // 需要在 app.json 里配置权限相关设置
  "permission": {
      "scope.userLocation": {
        "desc": "你的位置信息将用于小程序位置接口的效果展示" // 高速公路行驶持续后台定位
      }
   }
  ```

* wx.stopLocationUpdate 关闭监听实时位置变化，前后台都停止消息接收。
* wx.startLocationUpdateBackground 开启小程序进入前后台时均接收位置消息，需引导用户开启授权。 

  ```javascript
  // 支持下面这种方式授权
  wx.authorize({scope: 'scope.userLocationBackground'})
  ```
  
* 微信开发者工具上方有清除缓存调试。

* wx.startwx.startLocationUpdate 开启小程序性进入前台时接受位置信息。
* wx.startLocationUpdate 开启小程序进入前台时接受位置信息。
* wx.openLocation 使用微信内置地图查看位置。

```javascript
wx.getLocation({
  type: 'gcj02', //返回可以用于wx.openLocation的经纬度
  success (res) {
    const latitude = res.latitude
    const longitude = res.longitude
    wx.openLocation({
      latitude,
      longitude,
      scale: 18
    })
  }
 })
```

* wx.onLocationChange(function callback) 

   监听实时地理位置变化事件，需结合 wx.startLocationUpdateBackground、wx.startLocationUpdate 使用。

* wx.offLocationChange

  取消监听实时地理位置变化事件。

* wx.getLocation

​       获取当前的地理位置、速度。用户离开小程序，无法调用。开启高精度定位，耗时会增加。

* wx.choosPoi 

  打开地图选择位置，支持模糊定位。支持精确到市和精确定位混选。

* wx.chooseLocation 打开地图选择位置。

##### 路由

* wx.switchTab 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面。
* wx.reLaunch 关闭所有页面，打开到应用内的某个页面。
* wx.redirectTo 关闭当前页面，跳转到应用内的某个页面，tabBar 除外。
* wx.navigateTo 保留当前页面，跳转到应用内的某个页面，tabBar 除外。可以接 events 页面通信。
* wx.navigateBack 关闭当前页，返回上一页面或多级页面。可通过 getCurrentPages 获取当前页面栈，决定返回几层。delta 返回的页面数，如果 delta 大于现有的页面数，则返回到首页。

* navigatorTo 跳转时，调用该方法的页面会被加入堆栈，而 redirectTo 方法则不会。

#####  EventChanel

* emit 触发一个事件
* on 持续监听一个事件
* once 监听一个事件一次，出发后失效
* off 取消监听一个事件，给出第二个参数时，只取消给出的监听函数，否则取消所有监听函数。

##### 发起请求

* wx.request()

##### 上传

* wx.uploadFile()

### 视图层

#### WXML

##### 数据绑定

```html
<view>{{ message }}</view>

<script>
  Page({
    data: {
      message: 'Hello MINA!'
    }
  })
</script>
```

* WXML 中的动态数据均来自对应 Page 的 data。

```html
<view id="item-{{id}}"> </view>
<view wx:if="{{condition}}"> </view>

<!-- 布尔值也要用 Mustache -->
<checkbox checked="{{false}}"> </checkbox>

<!-- Mustache 内支持三元运算符，算数运算，逻辑判断，字符串运算，数据路径运算 -->
<!-- 可以组合构建新的对象或数组 -->

<view wx:for="{{[zero, 1, 2, 3, 4]}}"></view>
<template is="objectCombine" data="{{for: a, bar: b}}"></template>
<!-- 也可用展开运算符将对象展开 -->
```

* 注意：如果花括号和引号之间有空格， 会被解析为字符串。

```html
<view wx:for="{{[1,2,3]}} ">
  {{item}}
</view>

<view wx:for="{{[1,2,3] + ' '}}">
  {{item}}
</view>
```

##### 列表

```html
<view wx:for="{{ array }}"> {{item}} </view>

<script>
   Page({
     data: {
       array: [1,2,3,4,5]
     }
   })
</script>

<!-- wx:for 控制属性绑定一个数组 下标变量名默认为 index,数组当前项的变量名默认为 item 可以指定名称 -->
<view wx:for="{{array}}" wx:for-index="idx" wx:for-item="itemName">
  {{idx}}: {{itemName.message}}
</view>
```

```html
<!-- v-for 可以嵌套 -->
<view wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="i">
  <view wx:for="{{[1, 2, 3, 4, 5, 6, 7, 8, 9]}}" wx:for-item="j">
    <view wx:if="{{i <= j}}">
      {{i}} * {{j}} = {{i * j}}
    </view>
  </view>
</view>
```

* block 可以使用 wx:for 以渲染一个包含多节点的结构快。
* wx:key 列表项位置动态改变，并且希望列表中的项目保持自己的特征和状态。执行唯一标识符。
  * 字符串：代表 array 中 item 的某个 propery，该 property 的值需要是列表中唯一的字符串或数字。且不能动态改变。
  * 保留关键字 this 代表 item 本身。 需要 item 本身是一个唯一的字符串或者数字。
* 注意事项: wx:for 的值为字符串时，会将字符串解析为字符串数组。

##### 条件渲染

```html
<view wx:if="{{view == 'WEBVIEW'}}"> WEBVIEW </view>
<view wx:elif="{{view == 'APP'}}"> APP </view>
<view wx:else="{{view == 'MINA'}}"> MINA </view>

<script>
  Page({
    data: {
      view: 'MINA'
    }
  })
</script>
```

* 可以用在 block 上。block 仅仅是一个包装元素，不会在页面中做任何渲染，只接受控制属性。
* wx:if  之中的模板可能包含数据绑定，所以条件值切换时，框架有一个局部渲染的过程，
* wx:if 是惰性的。
* hidden 始终渲染， 只是简单的控制显示与隐藏。

##### 模板

```html
<template name="staffName">
  <view>
    FirstName: {{firstName}}, LastName: {{lastName}}
  </view>
</template>

<template is="staffName" data="{{...staffA}}"></template>
<template is="staffName" data="{{...staffB}}"></template>
<template is="staffName" data="{{...staffC}}"></template>

<script>
    Page({
      data: {
    	staffA: {firstName: 'Hulk', lastName: 'Hu'},
    	staffB: {firstName: 'Shang', lastName: 'You'},
    	staffC: {firstName: 'Gideon', lastName: 'Lin'}
  	  }
	})
</script>
```

#### WXSS

* rpx: 可以根据屏幕宽度进行自适应。屏幕宽度 750 rpx。 iphone6上，屏幕宽度为 375 px, 共有 750 个 物理像素。750 rpx = 375 px =750 物理像素，1rpx =0.5px = 1物理像素。
* 样式导入。 `import` 加相对路径。
* 内联样式。静态的样式统一写到 class 中。 style 接收动态的样式。在运行时会进行解析。

```html
<view style="color:{{color}};" />
<view class="normal_view" />
<!-- class 不需要加 . -->
```

#### 事件

##### 什么时事件

* 事件是视图层到逻辑层的通讯方式。
* 事件可以将用户可以将用户的行为反馈到逻辑层进行处理。
* 事件可以绑定到组件上，当达到触发事件，就会执行逻辑层中对应的事件处理函数。
* 小程序的回调无法像 Vue 一样传递参数，而是需要使用约定的一些组件属性来传递额外信息，如 `id`、`dataset`、`touches`。

##### 使用方式

```html
<view id="tapTest" data-hi="Weixin" bindtap="tapName"> Click me! </view>

<script>
  Page({
    tapName: function(event) {
      console.log(event)
    }
  })
</script>
```

```json
{
  "type":"tap",
  "timeStamp":895,
  "target": {
    "id": "tapTest",
    "dataset":  {
      "hi":"Weixin"
    }
  },
  "currentTarget":  {
    "id": "tapTest",
    "dataset": {
      "hi":"Weixin"
    }
  },
  "detail": {
    "x":53,
    "y":14
  },
  "touches":[{
    "identifier":0,
    "pageX":53,
    "pageY":14,
    "clientX":53,
    "clientY":14
  }],
  "changedTouches":[{
    "identifier":0,
    "pageX":53,
    "pageY":14,
    "clientX":53,
    "clientY":14
  }]
}
```

##### 绑定并阻止事件冒泡

* 除 bind 外，可以用 catch 来绑定事件。catch 会阻止事件向上冒泡。

##### 互斥事件绑定

* mut-bind 来绑定事件。一个 mut-bind 触发，如果事件冒泡到其他节点，其他节点的 mut-bind 绑定函数不会触发，bind 和 catch 绑定的函数仍然会触发。

##### 捕获阶段

* capture-bind、capture-catch。后者终端捕获阶段，取消冒泡阶段。

##### dataset

* data-element-type，最终会呈现为 event.currentTarget.dataset.elementType
* data-elementType，最终会呈现为 event.currentTarget.dataset.elementtype

```html
<view data-alpha-beta="1" data-alphaBeta="2" bindtap="bindViewTap"> DataSet Test </view>

<script>
  Page({
    bindViewTap:function(event){
      event.currentTarget.dataset.alphaBeta === 1 // - 会转为驼峰写法
      event.currentTarget.dataset.alphabeta === 2 // 大写会转为小写
    }
  })
</script>
```

##### mark

* 可以使用 mark 来识别具体触发事件的 target 节点。mark 还可以用于承载一些自定义数据。事件触发，事件冒泡路径上所有的 mark 会被合并，并返回给事件回调函数。

```html
<view mark:myMark="last" bindtap="bindViewTap">
  <button mark:anotherMark="leaf" bindtap="bindButtonTap">按钮</button>
</view>

<script>
Page({
  bindViewTap: function(e) {
    e.mark.myMark === "last" // true
    e.mark.anotherMark === "leaf" // true
  }
})
</script>
```

* mark 与 dataset 的区别：mark 会包含从触发事件的节点到根节点上所有的 mark 属性值。dataset 仅包含一个节点的 data- 属性值。
* 如何存在同名的 mark，父节点的 mark 会被子节点覆盖。
* 在自定义组件中接收事件时，mark 不会包含自定义组件外的节点的 mark。
* 不同于 dataset, 节点的 mark 不会做连字符和大小写转换。

##### touches 

* 是一个数组，每个元素为一个 Touch 对象。表示当前停留在屏幕上的触摸点。

#### 简易双向绑定

##### 双向绑定语法

```html
<input model:value="{{value}}" />
```

* 双向绑定的表达式有如下限制：只能是一个单一字段的绑定，不能计算组合。不支持 data 路径。

##### 自定义组件中传递双向绑定

```html
<!-- cusotm-component.js -->
Component({
  properties: {
    myValue: String
  }
})

<!-- custom-component.wxml -->
<input model:value="{{myValue}}" />

<custom-component model:my-value="{{pageValue}}" />
```

##### 在自定义组件中触发双向绑定更新

```javascript
Component({
  properties: {
    myValue: String
  },
  methods: {
    update: function() {
      // 更新 myValue
      this.setData({
        myValue: 'leaf'
      })
    }
  }
})
```

#### 基础组件

##### 类型属性

* Boolean 组件上写改属性，不管是什么值都被当做 `true`。只有组件上没有该属性时，属性值才为 `false`。如果属性值为变量，变量的值会被转换为 Boolean 类型。

* page-container 假页容器，页面内存在该容器时，用户返回，关闭该容器不关闭页面。

* scroll-view 可滚动视图区域。使用竖向滚动时，需要给一个 scroll-view 一个固定高度。

#### 获取页面上的节点信息

```javascript
const query = wx.createSelectorQuery()
```

## 小程序运行时

### 运行环境

* JavaScript 语法和 API 支持不一致：开发者可以通过开启 ES6 转 ES5 的功能 来规避。
* 小程序还内置了 Polyfill，来弥补 API的差异。
* wxss 渲染不一致。

### JavaScript 支持情况

#### 运行限制

* 不支持使用 eval 执行 JS 代码。
* 不支持使用 new Function 创建函数。
* Proxy 对象无法被 Polyfill。

#### Promise 时序差异

* ios 下的 Promise 是一个使用 setTimeout 模拟的 Polyfill。Promise 触发的任务为普通任务而非微任务。进而导致在  IOS 下的 Promise 时序和标准存在差异。

### 小程序运行机制

#### 小程序的生命周期

![小程序生命周期](https://res.wx.qq.com/wxdoc/dist/assets/img/life-cycle.5558d9eb.svg)

* 冷启动：第一次打开或销毁后再次打开，此时小程序需要重新加载启动，即冷启动。
* 热启动：如果用户已经打开过某小程序，没销毁是打开。
* 启动一般值冷启动，热启动被称为后台切前台。

* 挂起会停止小程序 JS 线程的执行，内存状态会保存。

* 开发者使用后台音乐播放、后台地理位置能力时，小程序可以在后台持续运行，不会进入到挂起状态。

#### 小程序冷启动页面

#### 小程序热启动页面

* 取决于启动场景带不带 path

#### 退出状态

* 销毁前，页面回调函数 onSaveExitState 会被调用。可以保留一些数据。

### 更新机制

## 自定义组件

## 创建自定义组件

> 一个自定义组件 由 json wxml wxss js 4个文件组成，要编写一个自定义组件，首先需要在 json 文件中将 component 字段设为 true。

```json
{
  "component": true
}
```

```html
<!-- 这是自定义组件的内部WXML结构 -->
<view class="inner">
  {{innerText}}
</view>
<slot></slot>
```

* 在组件 wxss 中不应该使用 ID 选择器、属性选择器和标签选择器。
* 自定义组件的 js 文件中。需要使用 Component 来注册组件。

```javascript
Component({
  properties: {
    // 这里定义了innerText属性，属性值可以在组件使用时指定
    innerText: {
      type: String,
      value: 'default value',
    }
  },
  data: {
    // 这里是一些组件内部数据
    someData: {}
  },
  methods: {
    // 这里是一个自定义方法
    customMethod: function(){}
  }
})
```

## 使用自定义组件

* 需要在页面的 json 文件中进行引导声明。

```json
{
  "usingComponents": {
    "component-tag-name": "path/to/the/custom/component"
  }
}
```

* app.json 中声明可为全局声明。

* 注意

  > 自定义组件的标签名只能是小写字母、中划线、下划线。
  >
  > 自定义组件可以引用自定义组件，也是 usingComponents 字段。
  >
  > 自定义组件和页面所载项目根目录名不能以“wx-” 为前缀，否则会报错。
  >
  > 使用 usingComponents 会使页面 this 对象原型链有差异。
  >
  > 使用后原型会不一致。
  >
  > 使用后会多一些方法，如 selectComponent。
  >
  > 使用 usingComponents, setDate 内容不会直接深复制。即 `this.setData({ field: obj })` 后 `this.data.field === obj` 。（深复制会在这个值被组件间传递时发生。）
  >
  > 新增或删除 usingComponents 定义段时重新测试一下。

### 组件模板和样式

#### 组件模板

* 组件模板的写法与页面模板相同。组件模板与组件数据结合生成的节点树。将被插入到组件的引用位置上。在组件模板中可以提供一个 `<slot>` 节点，用于承载组件引用时提供的子节点。

```html
<!-- 组件模板 -->
<view class="wrapper">
  <view>这里是组件的内部节点</view>
  <slot></slot>
</view>

<!-- 引用组件的页面模板 -->
<view>
  <component-tag-name>
    <!-- 这部分内容将被放置在组件 <slot> 的位置上 -->
    <view>这里是插入到组件slot中的内容</view>
  </component-tag-name>
</view>
```

#### 模板数据绑定

* 可以向子组件的属性动态传递数据。

```html
<!-- 引用组件的页面模板 -->
<view>
  <component-tag-name prop-a="{{dataFieldA}}" prop-b="{{dataFieldB}}">
    <!-- 这部分内容将被放置在组件 <slot> 的位置上 -->
    <view>这里是插入到组件slot中的内容</view>
  </component-tag-name>
</view>
```

* 这样的数据绑定只能传递 JSON 兼容数据。可以在数据中包含函数。

#### 组件 wxml 的 slot

* 默认情况，一个组件的 wxml 中只能有一个 slot, 需要使用多 slot 时，需要在组件 js 中声明。

```javascript
Component({
  options: {
    multipleSlots: true // 在组件定义时的选项中启用多slot支持
  },
  properties: { /* ... */ },
  methods: { /* ... */ }
})
```

* 多个 slot 用 name 来区分

```html
<!-- 组件模板 -->
<view class="wrapper">
  <slot name="before"></slot>
  <view>这里是组件的内部细节</view>
  <slot name="after"></slot>
</view>

<!-- 引用组件的页面模板 -->
<view>
  <component-tag-name>
    <!-- 这部分内容将被放置在组件 <slot name="before"> 的位置上 -->
    <view slot="before">这里是插入到组件slot name="before"中的内容</view>
    <!-- 这部分内容将被放置在组件 <slot name="after"> 的位置上 -->
    <view slot="after">这里是插入到组件slot name="after"中的内容</view>
  </component-tag-name>
</view>
```

#### 组件样式

* 组件对应 `wxss` 文件的样式，只对组件wxml内的节点生效。
* 注意

> 组件和引用组件的页面不能使用id选择器（`#a`）、属性选择器（`[a]`）和标签名选择器，请改用class选择器。
>
> 组件和引用组件的页面中使用后代选择器（`.a .b`）在一些极端情况下会有非预期的表现，如遇，请避免使用。
>
> 子元素选择器（`.a>.b`）只能用于 `view` 组件与其子节点之间，用于其他组件可能导致非预期的情况。
>
> 继承样式，如 `font` 、 `color` ，会从组件外继承到组件内。
>
> 除继承样式外， `app.wxss` 中的样式、组件所在页面的的样式对自定义组件无效（除非更改组件样式隔离选项）。

* `:host` 选择器  组件可以指定它所在节点的默认样式。

```css
/* 组件 custom-component.wxss */
:host {
  color: yellow;
}
```

```html
<!-- 页面的 WXML -->
<custom-component>这段文本是黄色的</custom-component>
组件样式隔离
```

#### 组件样式隔离

* 自定义组件样式只收到自定义组件 wxss 的影响。除非：

  * app.wxss 或页面的 wxss 中使用了标签名选择器来指定样式。这些选择器会影响到页面和全部组件。

  * 指定特殊的样式隔离选项 styleIsolation。

    ```javascript
    Component({
      options: {
        styleIsolation: 'isolated'
          //启用样式隔离 自定义组件内外 使用 class 指定的样式互不影响
      }
    })
    ```

    

#### 外部样式类

* 有时，组件希望接受外部传入的样式类。此时可以在 Component 中用 externalClasses 定义若干个外部样式类。样式类写在页面中而不是组件中。 

```javascript
/* 组件 custom-component.js */
Component({
  externalClasses: ['my-class']
})
```

```html
<!-- 组件 custom-component.wxml -->
<custom-component class="my-class">这段文本的颜色由组件外的 class 决定</custom-component>
```

#### 引用页面或父组件的样式

* 即使启用了样式隔离 isolate, 组件仍然可以在局部引用组件所载页面的样式或父组件的样式。

```css
// 页面 wxss 中定义了
.blue-text {
  color: blue;
}
```

```html
// 组件中可以使用 ~
<view class="~blue-text"> 这段文本是蓝色的 </view>
```

* 如果在一个组件的父组件 wxss 中定义了：

```css
.red-text {
  color: red;
}
```

* 在这个组件中可以使用 `^` 来引用这个类的样式

```html
<view class="^red-text"> 这段文本是红色的 </view>
```

#### 虚拟化组件节点

* 不希望节点本身可以设置样式，响应 flex 布局。而是希望滴定仪组件内部的第一层节点能够响应 flex 布局或者样式由自定义组件本身决定。

### Component 构造器

* properties data:{} lifetimes{ attached:function(){}, moved:function(){}; detached: function() {} }  pageLifetimes: {}

### 组件间通信与事件

#### 组件间通信

* wxml 数据绑定。用于父组件向子组件的指定属性设置数据。仅能设置 JSON 兼容数据。
* 事件: 用于子组件向父组件传递数据，可以传递任意数据。
* 父组件可以通过 this.selectComponent 方法获取子组件实例对象。可以直接访问。

#### 监听事件

* 自定义组件可以触发任意的事件。

```html
<!-- 当自定义组件触发“myevent”事件时，调用“onMyEvent”方法 -->
<component-tag-name bindmyevent="onMyEvent" />
<!-- 或者可以写成 -->
<component-tag-name bind:myevent="onMyEvent" />

<script>
  Page({
    onMyEvent: function(e){
      e.detail // 自定义组件触发事件时提供的detail对象
    }
  })
</script>
```

#### 触发事件

* 自定义组件触发事件，需要使用 triggerEvent。

```html
<!-- 在自定义组件中 -->
<button bindtap="onTap">点击这个按钮将触发“myevent”事件</button>

<script>
 Component({
  properties: {},
  methods: {
    onTap: function(){
      var myEventDetail = {} // detail对象，提供给事件监听函数
      var myEventOption = {} // 触发事件的选项
      this.triggerEvent('myevent', myEventDetail, myEventOption)
    }
  }
}) 
</script>
```

* 触发事件选项
  * bubbles 事件是否冒泡。
  * composed 事件是否可以穿越组件边界，为false时，事件将只能在引用组件的节点树上触发，不进入其他任何组件内部。
  * capturePhase 事件是否拥有可捕获阶段。

#### 获取组件实例

可在父组件里调用 this.selectComponent('.my-component')，获取子组件的实例对象。

```javascript
// 父组件
Page({
  data: {},
  getChildComponent: function () {
    const child = this.selectComponent('.my-component');
    console.log(child)
  }
})
```

#### 组件生命周期

* 最重要的生命周期是 created attached detached，包含一个组件实例生命流程的最主要时间点。
* 组件实例刚创建好时，created 生命周期被触发。此时，组件数据 this.data就是在 `Component` 构造器中定义的数据 `data` 。 **此时还不能调用 `setData` 。** 通常情况下，这个生命周期只应该用于给组件 `this` 添加一些自定义属性字段。
* 在组件完全初始化完毕、进入页面节点树后， `attached` 生命周期被触发。此时， `this.data` 已被初始化为组件的当前值。这个生命周期很有用，绝大多数初始化工作可以在这个时机进行。
* 在组件离开页面节点树后， `detached` 生命周期被触发。退出一个页面时，如果组件还在页面节点树中，则 `detached` 会被触发。

##### 定义生命周期方法

* lifetimes。
* 组件所在页面生命周期。

#### 数据监听器

##### 使用数据监听器

```javascript
Component({
  attached: function() {
    this.setData({
      numberA: 1,
      numberB: 2,
    })
  },
  observers: {
    'numberA, numberB': function(numberA, numberB) {
      // 在 numberA 或者 numberB 被设置时，执行这个函数
      this.setData({
        sum: numberA + numberB
      })
    }
  }
})
```

##### 监听字段语法

* 数据监听器支持监听属性或内部数据的变化，可以同时监听多个。一次 setData 最多触发每个监听器一次。

```javascript
Component({
  observers: {
    'some.subfield': function(subfield) {
      // 使用 setData 设置 this.data.some.subfield 时触发
      // （除此以外，使用 setData 设置 this.data.some 也会触发）
      subfield === this.data.some.subfield
    },
    'arr[12]': function(arr12) {
      // 使用 setData 设置 this.data.arr[12] 时触发
      // （除此以外，使用 setData 设置 this.data.arr 也会触发）
      arr12 === this.data.arr[12]
    },
  }
})
```

* 如果需要监听所有子数据字段的变化，可以使用通配符 `**` 。

```javascript
'some.field.**': function(field) {
  // 使用 setData 设置 this.data.some.field 本身或其下任何子数据字段时触发
  // （除此以外，使用 setData 设置 this.data.some 也会触发）
  field === this.data.some.field
},
```

* 特别地，仅使用通配符 `**` 可以监听全部 setData 。
* 数据监听器监听的是 setData 涉及到的数据字段，即使这些数据字段的值没有发生变化，数据监听器依然会被触发。
* 如果在数据监听器函数中使用 setData 设置本身监听的数据字段，可能会导致死循环，需要特别留意。
* 数据监听器和属性的 observer 相比，数据监听器更强大且通常具有更好的性能。

