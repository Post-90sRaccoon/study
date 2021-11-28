# 常用 CSS

* 单行文字超出省略。

```css
.ellipsis() {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
```

* 多行文字超出省略。

```css
.multi-ellipsis(@lines: 1) {
  display: -webkit-inline-box;
  overflow: hidden;
  text-overflow: ellipsis;
  word-break: break-all;
  -webkit-line-clamp: @lines;
  -webkit-box-orient: vertical;
}
```

