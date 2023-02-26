# Mouse Move Shadow

## 摘要

本篇实现鼠标在图片四周移动，分别产生不同效果，运用到`offset`的操作。

## 重点

>01.获取元素，设置监听

```javascript
  const hero = document.querySelector('.hero');
  const text = hero.querySelector('h1');

  function shadow(e){
	console.log(e)
  }

  hero.addEventListener('mousemove', shadow);
```


>02.定义变量

  ```javascript
  const xxx = a.aaa;
  const yyy = a.bbb;
  //可简化为
  const { aaa:xxx, bbb:yyy} = a
  ```

- `hero.offsetWidth`:显示`.hero`宽度(不会因为鼠标移动而改变)。

- `e.offsetX 及 e.offsetY`才是鼠标移动过程中的的位置。
- 但是这里有一个问题：当鼠标移动到`h1`(const text)的区域时，会只计算`h1`内的坐标，而移出`h1`后又会恢复正常。所以计算`h1`内的坐标时需要加上`e.target.offsetLeft`及`e.target.offsetTop`

```javascript
function shadow(e){
	const { offsetWidth : width, offsetHeight : height} = hero;
    let {offsetX : x, offsetY : y } = e;
    console.log(x, y)
}
```



- `this` vs `e.target`
  - `this(e.currentTarget)`：在javascript中，若`DOM element`被事件绑定，`this`表示的是被绑定事件的那个元素，不是子元素；
  - `e.target`：则是表示被触发的那个`Dom element`。若事件被绑定在父元素，而触发的是子元素时，会回传子元素的内容。

```javascript
  function shadow(e){
    const { offsetWidth : width, offsetHeight : height} = hero;
    let {offsetX : x, offsetY : y } = e;

    if (this !== e.target){
      x = x + e.target.offsetLeft;
      y = y + e.target.offsetTop;
    }
  }
```

>03.设置动画

- 定义`const walk`，作用是为整个`hero`提供动画显示的基准点。
  - 以`const walk = 100`为例， (实际的位置/全部的长度)*100 - (100 / 2)，这样会以中心点为(0.0)，左上及右下分別为(-50, -50)及(50,50)。
- 用`text.style.textShadow`显示动画，参数分别表示:`x, y, blur, color(rgba)`。
- `Math.round()`：取整。

```javascript
function shadow(e){
    const { offsetWidth : width, offsetHeight : height} = hero;
    let {offsetX : x, offsetY : y } = e;

    if (this !== e.target){
      x = x + e.target.offsetLeft;
      y = y + e.target.offsetTop;
    }

    const xWalk = Math.round(( x / width *walk) - (walk / 2));
    const yWalk = Math.round(( y / height *walk) - (walk / 2));

    text.style.textShadow = `
      ${xWalk}px ${yWalk}px 0 rgba(255,0,255,0.7),
      ${xWalk * -1}px ${yWalk}px 0 rgba(0,255,255,0.7),
      ${yWalk}px ${xWalk * -1}px 0 rgba(0,255,0,0.7),
      ${yWalk * -1}px ${xWalk}px 0 rgba(0,0,255,0.7)
    `;
  }
```


