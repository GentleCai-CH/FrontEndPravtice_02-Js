# Follow Along Nav Link

## 摘要

本篇涉及动画效果，当鼠标移动到`a`上时，加入`span`，并在`span`加入style，做出白色背景动画效果。

## 重点

>01.监听事件`(mouseenter)`。

- `mouseenter`：鼠标进入时触发。`mouseover` vs `mouseenter`:[比较](http://api.jquery.com/mouseover/)
  - `mouseenter`：只有鼠标进入时的元素会触发。
  - `mouseover`：元素的子元素被指到时也会触发。


```javascript
const triggers = document.querySelectorAll('a');
const highlight = document.createElement('span');
highlight.classList.add('highlight');
document.body.appendChild(highlight);

function highlightLink(){
  console.log(this);
}
triggers.forEach(a => a.addEventListener('mouseenter', highlightLink));
```

在`body`的最底部可以看到`<span class="highlight"></span>`，以及当鼠标移动到`a`元素上时，可以在console可以看到该元素。

>02.动画效果

- `this.getBoundingClientRect()`回传`DOMRect对象`，可以获取当前元素的位置。[參考](https://developer.mozilla.org/en-US/docs/Web/API/DOMRect)
- 
- `translate(x, y)`：元素平移。
- 
  - width：元素宽度。( = right - left)
  - height：元素高度。( = botton - top)
  - top：元素顶部距离。(=y or y+height)
  - left：元素左边距离。(=x or x+width)
  - botton：元素底边距离。(=y or y+height)
  - right元素右边距离。(=x or x+width)
  - x：该元素的x坐标
  - y：该元素的y坐标

```javascript
    function highlightLink(){
      // console.log(this);
      const linkCoords = this.getBoundingClientRect();
      // console.log(linkCoords);

      highlight.style.width = `${linkCoords.width}px`;// 在span内加入style.width
      highlight.style.height = `${linkCoords.height}px`;// 在span内加入style.height
      highlight.style.transform = `translate(${linkCoords.left}px, ${linkCoords.top}px)`;// 在span内加入style.transform效果。
    }
```

此时可以看到效果

但是当文档下移时位置发生改变!原因是`getBoundingClientRect()`获取的是窗口的相对位置，
还需要加上window的滑动距离。`DOMRect.left + window.scrollX`及`DOMRect.left + window.scrollY`

```javascript
    function highlightLink(){
      // console.log(this);
      const linkCoords = this.getBoundingClientRect();
      console.log(linkCoords);
      const coords = {
        width : linkCoords.width,
        height : linkCoords.height,
        top : linkCoords.top + window.scrollY,
        left : linkCoords.left + window.scrollX
      };

      highlight.style.width = `${coords.width}px`;
      highlight.style.height = `${coords.height}px`;
      highlight.style.transform = `translate(${coords.left}px, ${coords.top}px)`;
    }
```


> 完整代码

```javascript
  <script>
    const triggers = document.querySelectorAll('a');
    const highlight = document.createElement('span');
    highlight.classList.add('highlight');
    document.body.appendChild(highlight);

    function highlightLink(){
      // console.log(this);
      const linkCoords = this.getBoundingClientRect();
      console.log(linkCoords);
      const coords = {
        width : linkCoords.width,
        height : linkCoords.height,
        top : linkCoords.top + window.scrollY,
        left : linkCoords.left + window.scrollX
      };

      highlight.style.width = `${coords.width}px`;
      highlight.style.height = `${coords.height}px`;
      highlight.style.transform = `translate(${coords.left}px, ${coords.top}px)`;
    }

    triggers.forEach(a => a.addEventListener('mouseover', highlightLink));
  </script>
```

