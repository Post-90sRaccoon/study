# 日常遇到的问题

* antd 表单验证。`rules: [{ ...$validator.required(), type: 'array' }] `validator.required 返回一个对象。需要指定 type。
* 父组件给子组件传递异步请求的值，没拿到值，子组件已经渲染的问题。
    * 子组件 watch 传进来的值。
    * 组件 用 v-if 控制渲染子组件，没值直接不让他渲染。

