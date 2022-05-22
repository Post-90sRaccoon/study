# backdrop-filter

### 与 filter 的区别

* `background-filter` 是让当前元素所在区域后面的内容模糊灰度或高亮之类，要想看到效果，需要元素本身半透明或者完全透明。 `filter` 是让当前元素自身有效果。

### 毛玻璃效果的实现

```css
.card {
  backdrop-filter: blur(10px);
  background-color: rgba(255,255,255,0.72);
  overflow:hidden; // 隐藏可能出现的白边
}
```

### 下拉菜单毛玻璃效果

```css
list {
  background: rgba(255, 255, 255, .75);
  -webkit-backdrop-filter: blur(5px);    
  backdrop-filter: blur(5px);  
}

list:hover {
  background: rgba(255, 255, 255, .3);
}
```

### fallback

```css
.some-class-zxx {
    background-color: #fff;  
}
@supports (-webkit-backdrop-filter:none) or (backdrop-filter:none) {
    .some-class-zxx {
        background: hsla(0, 0%, 100%, .75);
        -webkit-backdrop-filter: blur(5px);    
        backdrop-filter: blur(5px);   
    }
}
```

## filter

### 灰度

```css
.card {
  filter: grayscale(100%); 
}
// 点亮徽章一张彩图即可
```

### 径向模糊，局部模糊

```html
  <div class="box-blur">
    <img src="https://www.zhangxinxu.com/study/201903/css-idea/example.jpg" class="radial-blur">
    <img src="https://www.zhangxinxu.com/study/201903/css-idea/example.jpg">
</div>
<p class="box-blur">
  <img src="https://www.zhangxinxu.com/study/201903/css-idea/example.jpg" class="local-blur">
  <img src="https://www.zhangxinxu.com/study/201903/css-idea/example.jpg">
</p>
```

```css
.box-blur {
  width: 256px;
  height: 192px;
  position: relative;
  overflow: hidden;
}
.radial-blur {
  filter: blur(20px);
  -webkit-mask-image: radial-gradient(transparent, transparent 10%, black 60%);
  transform: scale(1.2);
}
.local-blur {
  filter: blur(20px);
  -webkit-mask: no-repeat center;
  -webkit-mask-image: linear-gradient(black, black), linear-gradient(black, black);
  -webkit-mask-size: cover, 60px 60px;
  -webkit-mask-composite: exclude;
  -webkit-mask-composite: source-out;
  transform: scale(1.1);
}
img {
  width: 100%;
  display: block;
 }
.local-blur,
.radial-blur {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
 }
```

### 图标变色

* `filter: brightness(100)` 亮度高，就成白色了，低就成黑色了。

### 色彩流动

```html
<div class="flow-colorful"></div>
```

```css
.flow-colorful {
    max-width: 600px;
    height: 150px;
    background: linear-gradient(to right, red, orange, yellow, green, cyan, blue, purple);
    animation: hue 6s linear infinite;
}
@keyframes hue {
  from { filter: hue-rotate(0deg); }
  to { filter: hue-rotate(360deg); }
}

/* 文字流动 */
.font {
     font-size: 100px;
    -webkit-background-clip: text;
    color: transparent;
    background-image: linear-gradient(to right, red, yellow, lime, aqua, blue, fuchsia);
    animation: hue 6s linear infinite;
}
```





