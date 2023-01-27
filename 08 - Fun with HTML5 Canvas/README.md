# Canvas

##  摘要

本篇主要通过Html的`canvas`标签搭配Javascript做出画布效果。包括:颜色变化(`hsl`)及画笔粗细变化。

## 重点

> 01.首先定义画布的大小

- 用JS取到`canvas`标签后 ，
- 需要先设定画布的内容 ，使用`getContext('2d')`定义为2d绘图。
- 然后设置画布范围，`window.innerWidth`及`window.innerHeight`，没有设置范围时将使用html范围。

  ```javascript
  const canvas = document.querySelector('#draw');
  const ctx = canvas.getContext('2d');
  canvas.width = window.innerWidth;
  canvas.height = window.innerHeight;
  ```

> 02.定义画布的显示方式

- Canvas有许多属性，可參考[Canvas](http://www.w3school.com.cn/tags/html_ref_canvas.asp)，这里设定4种属性。

  - `ctx.strokeStyle`定义画笔颜色，
  - `ctx.lineJoin`定义两线相交时的拐角，
  - `ctx.lineCap`定义结束端点样式。
  - `ctx.lineWidth`定义画笔粗细。

  ```javascript
  ctx.strokeStyle = '#BADA55';
  ctx.lineJoin = 'round'; //round为圆弧。
  ctx.lineCap = 'round'; 
  ctx.lineWidth = 100;
  ```

> 03.定义画布的起始方式。

- 利用`isDrawing`等于`true`表示正在绘制，`false`表示不再绘制。

  ```javascript
  let isDrawing = false; //一开始为false
  ```

> 04.设置事件监听，驱动绘制。

- 用到了`mousedown`鼠标按下， `mousemove`鼠标移动，`mouseup`鼠标松开，及`mouseout` 鼠标离开窗口等事件类型。
  - 鼠标按下后`isDrawing`变为`true`，开始绘制。
  - 鼠标松开后`isDrawing`变为`false`，绘制结束。
  - 离开窗口时`isDrawing`变为`false`，取消绘制。

  ```javascript
  canvas.addEventListener('mousedown', ()=>isDrawing = true); //开始绘制
  canvas.addEventListener('mousemove', draw);//绘制中
  canvas.addEventListener('mouseup', ()=>isDrawing = false);//绘制结束
  canvas.addEventListener('mouseout', ()=>isDrawing = false);//取消绘制
  ```

> 05.定义监听处理函数，即进行绘制。

- 定义`draw`方法。首先判定`isDrawing`是否为`true`，为`false`则返回。

  ```javascript
  function draw(e){
    if (!isDrawing) return; 
    console.log(e); //可以打开使用者工具看看是否有返回坐标信息，若有则表示正确!
  }
  ```

> 06.定义画布的内容，用到4个参数。

  - `ctx.beginPath()`绘制开始。
  - `ctx.moveTo(a,b)`起始位置。
  - `ctx.lineTo(a,b)`终点位置。
  - `ctx.stroke()`   已绘制的路径。

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

- `hsl`是一個色彩表示的方式`hsl(hue, Saturation, Lightness)`。

  - `hue`代表顏色的度數0-360，0是紅色 ，120是綠色，240是藍色；
  - `Saturation`代表灰階程度，0%為灰黑，100%為彩色，一般設置為100%。
  - `Lightness`為亮度，0%代表黑，100%代表白，一般設置為50%。

  ```javascript
  let hue = 0;

  function draw(e){
    ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`;//重新定義顏色
    ...
    hue++;
    if(hue>=360){
      hue = 0; //若++到360自動歸零
    }
  }
  ```

> 08.顏色定義完了， 應該可以看到效果了吧?接下來要來定義寬度囉，這邊的處理方式是由細到粗，並回歸到細。

- 定義`direction`為粗細的參數，並定義其在`draw`中的變化。

- 當`direction`為`ture`時，`ctx.lineWidth`遞增，當增加到100時把`direction`改成`false`。

- 當`direction`為`false`時，`ctx.lineWidth`遞減，當撿到1時把`direction`改成`true`。

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
