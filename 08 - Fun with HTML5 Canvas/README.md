# [Canvas](http://www.w3school.com.cn/tags/html_ref_canvas.asp)

##  摘要

本篇主要通过Html的`canvas`标签搭配Javascript做出画布效果。包括:颜色变化(`hsl`)及画笔粗细变化。

## 重点

> 01.首先定义画布的大小

- 用JS取到`canvas`标签后 ，
- 需要先设定画布对象 ，使用`element.getContext('2d')`定义为2d对象。
- 然后设置画布大小，`window.innerWidth`及`window.innerHeight`，没有设置范围时将使用html范围。

  ```javascript
  const canvas = document.querySelector('#draw');
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  ```

> 02.定义画布的样式

- Canvas有许多属性，这里设定4种属性。

  - `ctx.strokeStyle`定义画笔颜色，
  - `ctx.lineJoin`定义两线相交时的拐角类型。miter 默认,尖角。bevel 斜角。round	 圆角。
  - `ctx.lineCap`定义结束端点样式。butt	默认。每个末端添加平直边缘。round	每个末端添加圆形线帽。square	每个末端添加正方形线帽。
  - `ctx.lineWidth`定义画笔粗细。

  ```javascript
  ctx.strokeStyle = '#BADA55';
  ctx.lineJoin = 'round'; //round为圆弧。
  ctx.lineCap = 'round'; 
  ctx.lineWidth = 100;
  ```

> 03.定义画布的起始状态。

- 用`isDrawing`等于`true`表示正在绘制，`false`表示没有绘制。

  ```javascript
  let isDrawing = false; //一开始为false
  ```

> 04.设置事件监听，驱动绘制。

- 用到了`mousedown`鼠标按下， `mousemove`鼠标移动，`mouseup`鼠标松开，及`mouseout` 鼠标离开窗口等事件类型。
  - 鼠标按下后`isDrawing`变为`true`，开始绘制。
  - 鼠标移动时，调用绘制函数。
  - 鼠标松开后`isDrawing`变为`false`，绘制结束。
  - 离开窗口时`isDrawing`变为`false`，取消绘制。

  ```javascript
  canvas.addEventListener('mousedown', ()=>isDrawing = true); //开始绘制
  canvas.addEventListener('mousemove', draw);//绘制中
  canvas.addEventListener('mouseup', ()=>isDrawing = false);//绘制结束
  canvas.addEventListener('mouseout', ()=>isDrawing = false);//取消绘制
  ```

> 05.定义监听处理函数，即绘制函数。

- 定义`draw`方法。首先判定`isDrawing`是否为`true`，为`false`则返回。

  ```javascript
  function draw(e){
    if (!isDrawing) return; 
    console.log(e); //可以打开使用者工具看看是否有返回坐标信息，若有则表示正确!
  }
  ```

> 06.定义画布的内容，用到4个参数。
- 
  - `ctx.beginPath()`绘制开始。
  - `ctx.moveTo(a,b)`起始位置。
  - `ctx.lineTo(a,b)`终点位置。
  - `ctx.stroke()`   进行路径绘制。

- 先定义最后的位置为`lastX, lastY`。`e.offsetX`表示事件当前坐标，可以定义`e.offsetX, e.offsetY`为每次的起始位置。

  ```javascript
  let lastX = 0;
  let lastY = 0;

  function draw(e){
    if (!isDrawing) return;
    ctx.beginPath();
    ctx.moveTo(lastX, lastY);
    ctx.lineTo(e.offsetX, e.offsetY); //画到的位置。
    ctx.stroke();
  }

  canvas.addEventListener('mousedown', (e)=> {
    isDrawing = true;
    [lastX, lastY] = [e.offsetX, e.offsetY];
  }); //开始绘制
  ```

- 会发现，绘制路径都是以同一个点为起始位置，所以需要动态更新起始位置，在`draw`方法内加入`[lastX, lastY] = [e.offsetX, e.offsetY];`更新起始位置。

  ```javascript
  function draw(e){
    if (!isDrawing) return;
    ctx.beginPath();
    ctx.moveTo(lastX, lastY);
    ctx.lineTo(e.offsetX, e.offsetY); //画到的位置。
    ctx.stroke();
    [lastX, lastY] = [e.offsetX, e.offsetY];
  }
  ```

> 07.设置颜色变化

- `hsl`是一种色彩表示方式`hsl(hue, Saturation, Lightness)`。

  - `hue`表示色调，0-360，0是红色 ，120是绿色，240是蓝色；
  - `Saturation`表示饱和度，0%为灰黑，100%为彩色，一般设置为100%。
  - `Lightness`表示亮度，0%代表黑，100%代表白，一般设置为50%。

  ```javascript
  let hue = 0;

  function draw(e){
    ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`;//重新定义颜色
    ...
    hue++;
    if(hue>=360){
      hue = 0; //若++到360归零
    }
  }
  ```

> 08.定义画笔粗细变化。

- 定义`direction`变量，判断是否由细到粗变化。

- 当`direction`为`ture`时，`ctx.lineWidth`递增，增加到100时把`direction`改成`false`。

- 当`direction`为`ture`时，`ctx.lineWidth`递减，减到1时，把`direction`改成`true`。

  ```javascript
  let direction = true;

  function draw(e){
    ...
    if(ctx.lineWidth >= 100 || ctx.lineWidth <= 1){
      direction = !direction;
    }
    if(direction){
      ctx.lineWidth++
    }else{
      ctx.lineWidth--
    }
  }
  ```
