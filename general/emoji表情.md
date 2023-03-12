# 前端处理特殊字符和 emoji 表情

## 问题

* 因为 UTF-8 编码有可能是两个、三个、四个字节。
    Emoji 表情是4个字节，而 MySQL 的 utf8 编码最多 3 个字节，所以数据插不进去。

## 后端处理

* 在后端存入数据库的时候,将 MySQL 的编码从 utf8 转换成 utf8mb4。

## 前端处理

* 在前端进行正则验证，当提交表情的时候进行排除。

```javascript
var reg = /[\uD83C[\uDF00-\uDFFF]|\uD83D[\uDC00-\uDE4F]/
var reg_mark = /[`~!@#$%^&*()+=|{}':;',/\/\[\].<>/?~！@#￥%……&*（）——+|{}【】‘；：”“’。，、？]/
var reg = /<a?:.+?:\d{18}>|\p{Extended_Pictographic}/gu
/* 前面是自定义 emoji */
```

* NPM 包 emoji-regex

```javascript
const emojiRegex = require('emoji-regex')
const regex = emojiRegex()
const fmt_str = str.replace(regex, (p) => `emoji(${p.codePointAt(0)})`) 
console.log(fmt_str) // hello world emoji(129315)emoji(128175)!emoji(128588)

const emojiDecodeRegex = /emoji\(\d+\)/g
const ori_str = fmt_str.replace(emojiDecodeRegex, p => {
  const filterP = p.replace(/[^\d]/g, '')
  return String.fromCodePoint(filterP)
})
console.log(ori_str) // hello world 🤣💯!🙌
```

* 将输入的 emoji 转为 span 标签，赋予相应的 class，找一张所有 emoji 制成的图片。
* 转换成八进制实体字符

```html
<p> 0x1F602 => &#128512;</p>  // 0x1F602 => 😂
<p> 0x597d=> &#22909;</p>  // 0x597d => 好
```

```javascript
//把utf16的emoji表情字符进行转码成八进制的字符function utf16toEntities(str) {   // 检测utf16字符正则,只要落在0xD800到0xDBFF的区间，就要连同后面2个字节一起读取。
   // 类似的问题存在于所有的JavaScript字符操作函数
   var patt = /[\ud800-\udbff][\udc00-\udfff]/g;
    return str.replace(patt, function (char) {
        var H, L, code;        if (char.length === 2) {
            H = char.charCodeAt(0); // 取出高位  
            L = char.charCodeAt(1); // 取出低位  
            code = (H - 0xD800) * 0x400 + 0x10000 + L - 0xDC00; // 转换算法,知道这回事就行了~
            return "&#" + code + ";"; // HTML实体符
        } else {
            return char;
        }
    });
}

//将编码后的八进制的emoji表情重新解码成十六进制的表情字符
function entitiesToUtf16(str) {
    return str.replace(/&#(\d+);/g, function (match, dec) {
        let H = Math.floor((dec - 0x10000) / 0x400) + 0xD800;
        let L = Math.floor(dec - 0x10000) % 0x400 + 0xDC00;
        return String.fromCharCode(H, L);
    });
}
```

[CSS emoji](https://www.zhangxinxu.com/wordpress/2020/03/css-emoji-opentype-svg-fonts/)

