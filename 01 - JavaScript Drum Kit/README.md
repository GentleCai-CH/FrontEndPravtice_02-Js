# Drum kit

### 重点

```
window.addEventListener('keydown', playsound);
```


1. 通过 按键事件`keydown`事件类型 触发函数，
利用每个按键的`event.code`值，和 属性选择器`audio[data-key="${e.code}"]` 找到相应的audio元素，播放对应音频`audio.play()`。

2. 重置 音频开始播放时间 `audio.currentTime = num`
3. `transitionend`事件类型:  `transition`结束的事件  执行callback内容。
4. event.propertyName !== 'transform':   没有 transform css样式属性的事件？！？
5. Element.classList： 利用`event.target.classList.add('playing')`，添加样式类；同理用`event.target.classList.remove('playing')`移除样式类。

### 补充

1. flex:
	- align-items : center; //垂直居中
	- justify-content:center; //水平居中

2. transition过渡 参数: property duration timing-function delay;

  - Ex:transition:all 0.07s ease
  - 
	- property:有width, color...
	- timing function有:
		- ease          cubic-bezier(0.25, 0.1, 0.25, 1.0)
		- liner         cubic-bezier(0.0, 0.0, 1.0, 1.0)
		- ease-in       cubic-bezier(0.42, 0.0, 1.0, 1.0)
		- ease-out      cubic-bezier(0.0, 0.0, 0.58, 1.0)
		- ease-in-out   cubic-bezier(0.42, 0.0, 0.58, 1.0)
