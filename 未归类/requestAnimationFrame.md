[requestAnimationFrame](https://developer.mozilla.org/zh-CN/docs/Web/API/window/requestAnimationFrame)

```html
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <p style="margin-top:200px;text-align: center;">
    <img src="" style="border: 2px solid red; position: relative" alt="cat">
  </p>
  <script>
    var cat = document.querySelector('img')
    var angle = 0
    var lastTime = null

    function animate(time) {
      if (lastTime != null) {
        angle += (time - lastTime) * 0.001
      }//按照时间 不同刷新率相同速度 离开也会运行 定死角度离开还在原地
      lastTime = time
      cat.style.top = Math.sin(angle) * 20 + 'px'
      cat.style.left = Math.cos(angle) * 200 + 'px'
      requestAnimationFrame(animate)
    }
    requestAnimationFrame(animate) //让一个函数在未来某个时间点(下一帧之前)执行
  </script>
</body>

</html>
```

### 太阳和地球

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body {
      text-align: center;
      padding-top: 200px;
    }

    .sun {
      background-color: gold;
      border-radius: 99999px;
      border: 3px solid orange;
      width: 100px;
      height: 100px;
      text-align: center;
      line-height: 100px;
      font-size: 0;
      display: inline-block;
      position: relative;
    }

    .earth {
      vertical-align: middle;
      display: inline-block;
      background-color: aqua;
      border-radius: 9999px;
      width: 20px;
      height: 20px;
      line-height: 20px;
      border: 2px solid blue;
      position: relative;
    }

    .moon {
      vertical-align: middle;
      display: inline-block;
      background-color: orange;
      border-radius: 9999px;
      width: 5px;
      height: 5px;
      border: 1px solid yellow;
      position: relative;
    }
  </style>
</head>

<body>
  <div class="sun">
    <div class="earth">
      <div class="moon"></div>
    </div>
  </div>
  <script>
    let sun = document.querySelector('.sun')
    let earth = document.querySelector('.earth')
    let moon = document.querySelector('.moon')

    let lastTime = null
    let angle = 0
    requestAnimationFrame(function ani(time) {
      if (lastTime !== null) {
        angle += (time - lastTime) * 0.001
      }
      lastTime = time

      sun.style.top = Math.sin(angle) * 100 + 'px'
      sun.style.left = Math.cos(angle) * 100 + 'px'

      earth.style.top = Math.sin(angle * 2) * 200 + 'px'
      earth.style.left = Math.cos(angle * 2) * 200 + 'px'

      moon.style.top = Math.sin(angle * 2.4) * 70 + 'px'
      moon.style.left = Math.cos(angle * 2.4) * 70 + 'px'
      requestAnimationFrame(ani)
    })
  </script>
</body>

</html>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body {
      text-align: center;
      padding-top: 200px;
    }

    .sun,
    .earth,
    .moon {
      position: fixed;
    }

    .sun {
      background-color: gold;
      border-radius: 99999px;
      border: 3px solid orange;
      width: 100px;
      height: 100px;
      margin-top: -53px;
      margin-left: -53px;
      /* 改变定位中心 */
    }

    .earth {
      background-color: aqua;
      border-radius: 9999px;
      width: 20px;
      height: 20px;
      border: 2px solid blue;
      margin-top: -12px;
      margin-left: -12px;
    }

    .moon {
      background-color: orange;
      border-radius: 9999px;
      width: 6px;
      height: 6px;
      margin-top: -4px;
      margin-left: -4px;
      border: 1px solid yellow;
    }
  </style>
</head>

<body>
  <div class="sun"></div>
  <div class="earth"></div>
  <div class="moon"></div>

  <script>
    let sun = document.querySelector('.sun')
    let earth = document.querySelector('.earth')
    let moon = document.querySelector('.moon')

    let lastTime = null
    let angle = 0
    requestAnimationFrame(function ani(time) {
      if (lastTime !== null) {
        angle += (time - lastTime) * 0.001
      }
      lastTime = time

      sun.style.top = 300 + Math.sin(angle) * 100 + 'px'
      sun.style.left = 400 + Math.cos(angle) * 100 + 'px'

      earth.style.top = 300 + Math.sin(angle) * 100 + Math.sin(angle * 2) * 200 + 'px'
      earth.style.left = 400 + Math.cos(angle) * 100 + Math.cos(angle * 2) * 200 + 'px'

      moon.style.top = 300 + Math.sin(angle) * 100 + Math.sin(angle * 2) * 200 + Math.sin(angle * 2.4) * 70 + 'px'
      moon.style.left = 400 + Math.cos(angle) * 100 + Math.cos(angle * 2) * 200 + Math.cos(angle * 2.4) * 70 + 'px'
      requestAnimationFrame(ani)
    })
  </script>
</body>

</html>
```

