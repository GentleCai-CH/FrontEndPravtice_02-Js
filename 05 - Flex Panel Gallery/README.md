# Gallery

## 摘要
本文主要是利用 `flex` 的特性及 `transition` 的动画效果做出点击后的效果呈现。主要为css方面的内容。
可參考[flex.io](https://www.flex.io/)。


## 重点

>01.一开始时，图片为上到下排列呈现，所以第一步要做的事情是先让图片从左到右排列，并且等宽排列。
	- 首先对 `panels` 新增属性`display:flex`就可以达到左右排序的效果，
	- `panel`为`panels` 的子元素，通过在`panel`新增 `flex:1`，可以让子元素等宽填满。

```css
.panels{
	...
	display: flex;

.panel {
	...
	flex:1;
}
```

>02.接着要处理文字的效果问题，可以先把文字标注起来，以便观察效果：

```css
.panel > * {
	...
    border: 1px solid red;
    }

```

>03.接下來设置 文字居中，，且由上到下排列

	- `justify-content:center` 让文字水平置中，`align-items:center`让文字垂直居中。
	- 设置`display:flex`可以让该container使用flex的方式呈现。此时文字由左至右显示，通过改变主轴方向为垂直方向`flex-direction:column`，修改成由上到下排列。

```css
.panel {
	...
	display:flex;
	justify-content:center;
	align-items:center;
}

```

>04.接下來将.panel分为三等份排好。

	- 在`.panel > *`加入`flex:1 0 auto;`及水平垂直居中，让文字居中。

```css
.panel > * {
	...
	flex:1 0 auto;
	display:flex;
	justify-content:center;
	align-items:center;
}
```

>05.接下來设置动画，首先文字上下移动。

	- 选取到上下的文字方块，用`.panel > *:first-child`取到第一个 `.panel > *:last-child`取到最后一个，并分别设置`translateY(-100%)`向上移动，`translateY(100%)`向下移动。
	- 通过新增`open-active`样式，定义起始位置`translateY(0)` 。

```css
.panel > *:first-child {transform:translateY(-100%);}
.panel.open-active > *:first-child {transform:translateY(0);}
.panel > *:last-child {transform:translateY(100%);}
.panel.open-active > *:last-child {transform:translateY(0);}
```

>06.设置动画：让该项目使用`open`时，图片变大五倍。

	- 前面设置了`panel`的`flex :1`，所以此时只需修改插入`open`样式的`panel`执行五倍的效果即可`flex:5`。

```css
.panel.open {
	...
	flex:5;
}
```

> 07.最後用 Javascript 触发动画：當點擊後，該`panel`會圖片放大，接著文字會已動畫方式回覆。

- 首先先選取`.panel`的nodeList，並在每個nodeList放置監聽器，監聽效果為`click`後驅動`toggleOpen`放大圖片，放大圖片的方式是用`this.classList.toggle('open')`把`open`加到被選取的`panel`，接著等放大圖片的動畫過後透過`transitionend`驅動`toggleActive`呈現文字動畫效果，驅動後要先判定被監聽回傳的內容是不是包含`flex`名稱，依瀏覽器不同safari會回傳`flex`, 其他的則是`flex-flow`，最後判定後利用`this.classList.toggle('open-active')`直線文字效果。

```javascript
const panels = document.querySelectorAll('.panel');

function toggleOpen(){
	this.classList.toggle('open');
}

function toggleActive(){
	this.classList.toggle('open-active');
}


panels.forEach(panel => panel.addEventListener('click', toggleOpen));
panels.forEach(panel => panel.addEventListener('transitionend', toggleActive));
```

最後記得把文字框border消除，就完成今天的項目囉！
