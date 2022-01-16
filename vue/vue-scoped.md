# Scoped

* `<style scoped>` 的每一个选择器的最后一个选择器单元增加一个属性选择器。
* 将组件样式作用范围限制在了组件自身的标签。包含子组件的根标签。
* `>>>`,`/deep/`,`::v-deep`

### 父组件内引入子组件的情况

* 父组件的 `css` 加 `scoped`
* 父组件内出现的 `dom` 标签都会被添加同一个属性值，包括传给子组件的 `slot`。

``` css
.parent {
  color:#fff;
  /deep/ .child {
    color: #000;
    .inner {
      color: #0f0;
    }
  }
}

.parent[data-v-xxx] .child {}
.parent[data-v-xxx] .child .inner {}
```

