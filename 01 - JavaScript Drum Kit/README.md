# Drum kit

### 重点

1. 通过 按键事件`keydown`事件类型 触发函数，利用每个按键的`keyCode`值，对应到`data-key`属性，再通过 属性选择器`audio[data-key="${e.keyCode}"]` 对应dom元素，放出对应音频`audio.play()`。

```
window.addEventListener('keydown', playsound);
```

2. 重置 音频开始播放时间 `audio.currentTime = num`
3. 利用`selector.classList.add('playing')`，添加样式类；同理用`selector.classList.remove('playing')`移除样式类。
4. `transitionend`事件类型:  `transition`结束的事件  执行callback内容。

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
