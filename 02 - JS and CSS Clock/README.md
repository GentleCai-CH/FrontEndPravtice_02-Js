
# JS&CSS CLOCK

## 摘要

1. 设置 旋转中心 `transform-origin:100%`
	- 横线会以中心点当做轴心旋转(50%)，使用`transform-origin:100%`移动 轴心到最右侧（100%）。
	- 横线使用`transform:rotate(90deg)`变成竖线，其坐标轴跟随旋转，因此旋转中心依旧有效。
2. 使用日期对象`now = new Date()`
	- 使用日期对象取得当前时间`now = new Date()`, 使用`now.getHours()`方法、`now.getMinutes()`方法、`now.getSeconds()`方法取得时分秒,再计算相应的旋转角度。
3. 使用`setInterval()`定时执行函数`setInterval(setDate, 1000);`


```
const secondHand = document.querySelector('.second-hand');
function serDate(){
	const new = new Date();
	const seconds = now.getSeconds();
	const secondsDegree = ((seconds / 60)*360)+90;
	secondHand.style.transform = `rotate(${secondsDegree}deg)`
}
```
