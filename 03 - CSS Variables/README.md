
# Update CSS variable with JS

## 重点

1. 利用Javascript抓取的量(使用`change`监听颜色改变及`mousemove`事件监听range改变)，并更新css改变值。

2.  `this.dataset-xxx`
	- 选择所有`data-xxx`前缀的属性及其值。
	- 如使用`this.dataset.sizing`选择`data-sizing`属性的值，`data-sizing`存放的是`px`，因为取到值后需要加上单位`px`，颜色没有单位，所以也可以为空，避免报错。

3. `document.documentElement`指的是，调用该回调的元素
```
function updateData(e){
    const suffix = this.dataset.sizing || ' ';
    document.documentElement.style.setProperty(`--${this.name}`,this.value + suffix);
}
```

4. `NodeList` vs Array:
	- 使用`querySelectorAll('.controls input')`时会返回`NodeList`,，但是它不是Array， 可以使用`forEach` 方法，不能使用像是map, reduce等方法。
```
<div class="controls">
	<input >
	<input >
	<input >
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
4. `filter:blur()`滤镜，常见的还有`opacity`(透明度)。
