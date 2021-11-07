# 小程序踩坑

* 小程序的 picker date 中 end 和 start 属性格式只支持格式形如 2001-01-02 。2001/01/02 这种格式安卓机会失效，ios 正常。
* 小程序安卓的 button。 height = line height 文字不居中。使用 flex 进行文字居中。
* 小程序中 WXML 是不支持写表达式的，只能在 js 中计算好后再存在 data 中。
* 小程序 textarea placeholder 组件样式，第一次加载无效。可以 setData 一点值，然后置空。
* 小程序 wx:for，vue v-for。
* wx-key:"{{index}}"  错误 wx:key="index"。事件也不需要 {{}} 包裹。
* 快速多次点击 tab。会显示重复数据。问题原因，数据数组是 concat 的。异步请求导致返回顺序不一致。

```javascript
async fetchOrders() {
  const date = Date.now();
  console.log(`fetchOrders:before loadMore orders: ${this.data.orders} hasMore: ${this.data.hasMoreOrders} type: ${this.data.type} ${date}`);
  const newItems = await this.orderInfiniteList.loadMore();
  console.log(`fetchOrders:after loadMore orders: ${this.data.orders} hasMore: ${this.data.hasMoreOrders} type: ${this.data.type} ${date}`);
  const hasMoreOrders = this.orderInfiniteList.hasMore();
  console.log(`fetchOrders:after hasMore orders: ${this.data.orders} hasMore: ${this.data.hasMoreOrders} type: ${this.data.type} ${date}`);
  const orders = this.formatData(newItems);
  this.setData({
    hasMoreOrders,
    orders: this.data.orders.concat(orders),
  });
},
```

* 利用函数作用域，data 中 记录 newRequestDate 只取最新的 date。抛弃之前请求返回的数据。能用的前提， 发送的请求是同样的，返回的结果也是同样的，幂等操作。
* 如果连续触发触底，此方案行不通，不能抛弃之前的。可以采用 debounce，throttle，或者有的请求 concat 数组，有的直接赋值新数据等方案。
