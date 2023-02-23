# Custom Video Player

## 摘要

本篇会把chrome预设的播放器取消，实现定制化的视频播放器。

## 重点

>01. 首先获取每个元素

```html
<body>

   <div class="player">
     <video class="player__video viewer" src="./652333414.mp4"></video>

     <div class="player__controls">
       <div class="progress">
        <div class="progress__filled"></div>
       </div>
       <button class="player__button toggle" title="Toggle Play">►</button>
       <input type="range" name="volume" class="player__slider" min="0" max="1" step="0.05" value="1">
       <input type="range" name="playbackRate" class="player__slider" min="0.5" max="2" step="0.1" value="1">
       <button data-skip="-10" class="player__button">« 10s</button>
       <button data-skip="25" class="player__button">25s »</button>
     </div>
   </div>

  <script src="./scripts.js"></script>
</body>
```


```javascript
const player = document.querySelector('.player'); //最外层的div
const video = player.querySelector('.viewer'); //视频元素的div
const progress = player.querySelector('.progress'); //播放bar的外层div
const progressBar = player.querySelector('.progress__filled');//播放bar的div 
const toggle = player.querySelector('.toggle'); //播放按钮的div
const skipButtons = player.querySelectorAll('[data-skip]'); //两个`skip`的div
const ranges = player.querySelectorAll('.player__slider'); //声音和播放速度bar的div
```

>02. 控制播放和暂停

- 对`video` 及 `按钮(toggle)`点击时切换播放暂停。
- 方法`video.paused`可以判定视频是否在播放中，由此触发`play()`或`video[play]()`及`pause()`或`video[pause]()`方法。

```javascript
function togglePlay(){
	const method = video.paused ? 'play' : 'pause';
	video[method]();
}
video.addEventListener('click', togglePlay);
toggle.addEventListener('click', togglePlay);
```

>03. 改变按钮状态

- 播放`play()`或暂停`pause()`时会更改播放按钮状态。使用`toggle.textContent`更改内容。

```javascript
function updateButton(){
	const icon = this.paused ? '►' : '❚ ❚';
	toggle.textContent = icon;
}

video.addEventListener('play', updateButton);
video.addEventListener('pause', updateButton);
```

>04. 控制skip

- `skip`共有两个区域-10及+25，利用`data-xxx`属性，用`querySelectorAll([data-xxx])`进行获取，并逐一(`forEach`)添加事件监听，触发`skip`方法。
- 用`this.dataset.xxx`的方式获取`data-xxx`属性的值。
- 获取到的值是`string`，用`parseFloat()`强制转换成`number`。
- `video.currentTime`指视频当前播放时间。

```javascript
function skip(){
	const skip = this.dataset.skip;
	video.currentTime += parseFloat(this.dataset.skip);
}
skipButtons.forEach(button => button.addEventListener('click', skip));
```

>05. 控制声音和播放速度。

- 利用`change`或`mouseover`事件类型，获取`this.value`的值赋给`this.name`。
- `video[volumn]`及`video[playbackRate]`可以分別设置声音和播放速度。
- `input type="range"`设定范围:
  - `min`最小值
  - `max`最大值
  - `step`最小步进值
  - `value`预设值

```javascript
function handleRangeUpdate(){
	video[this.name] = this.value;
}
ranges.forEach(range => range.addEventListener('change', handleRangeUpdate));
ranges.forEach(range => range.addEventListener('mouseover', handleRangeUpdate));
```

>06. 处理播放bar的显示。

- 先获得进度条的比例位置：用`video.currentTime`获取当前值，除以`video.duration`(全长)，再乘以100。`const percent = (video.currentTime / video.duration) *100`
- 控制进度条比例：设置`progressBar.style.flexBasis = ${percent}%`;
- 使用`timeUpdate`或`progress`事件类型触发处理。

```javascript
function handleProgress(){
	const percent = (video.currentTime / video.duration) *100;
	progressBar.style.flexBasis = `${percent}%`;
}
video.addEventListener('timeupdate', handleProgress);
```

>07. 处理进度条拖拉。

- 利用`e.offsetX`取得当前该div的x值，除以div的全长`progress.offsetWidth`可以得到百分比，乘以`video.duration`获得当前播放时间。将其赋给`video.currentTime`。
- 设置`mousedown = false`当做flag。按下时(`mousedown)`设置为`true`，松开时`(mouseup)`设置为`false`，
- 移动进度条时(`mouseover`)进行判断，若`mousedown`为`true`则执行事件(scrub)。
- 点击进度条时同样执行事件(scrub)

```javascript
function scrub(e) {
  const scrubTime = (e.offsetX / progress.offsetWidth) * video.duration;
  video.currentTime = scrubTime;
}
let mousedown = false;
progress.addEventListener('click', scrub);
progress.addEventListener('mousemove', (e) => mousedown && scrub(e));
progress.addEventListener('mousedown', () => mousedown = true);
progress.addEventListener('mouseup', () => mousedown = false);
```

> 试试全屏展示按钮的实现

