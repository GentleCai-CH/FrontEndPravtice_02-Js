
# Update CSS variable with JS

## 重点

1. 利用Javascript抓取改變的量(使用change監聽顏色改變及mouseover事件監聽range)，並更新CSS的變數值。

4. NodeList vs Array:
	- 使用`querySelectorAll('.controls input')`时会返回NodeList,，但是它不是一個Array， 可以使用`forEach` 方法，不能使用像是map, reduce等方法。
```
<div class="controls">
	<input >
	<input >
	<input >
```

5. `this.dataset`:會出現該選取項目中所有`data-`的項目及值。舉例來說，如果要選取`data-sizing`的值，可以使用`this.dataset.sizing`，在範例中，`data-sizing`存放的是`px`，所以在取到目標的值之後，還需要加上`px`值才可以運作，不過顏色沒有單位，所以也可以為空，避免報錯。

```
function updateData(e){
    const suffix = this.dataset.sizing || ' ';
    document.documentElement.style.setProperty(`--${this.name}`,this.value + suffix);
}
```

## 补充

1. `:root`伪类
	- `:root` 表示 <html> 元素，除了优先级更高之外，与 html 选择器相同。用于声明全局 CSS 变量。
2. CSS 变量/自定义样式属性: 
	- 以`--`开头，`--variable`，还可以用在元素的style属性上
```css
\\宣告方式
:root{
	--base: #ffc600;
	--blur: 10px;
	--spacing: 10px;
}
\\使用方式
span.hl{
	color: var(--base);
}
img{
	padding: var(--spacing);
	filter: blur(var(--blur));
	background: var(--base);
}
```

3. `type="range"`类型的input，生成范围控件。`type="color"`类型的input，生成颜色选择器。
```html
<label for="blur">Blur:</label>
<input id="blur" type="range" name="blur" min="0" max="25" value="10" data-sizing="px">

<label for="base">Base Color</label>
<input id="base" type="color" name="base" value="#ffc600">
```
4. filter:blur()滤镜，常见的还有opacity(透明度)。
