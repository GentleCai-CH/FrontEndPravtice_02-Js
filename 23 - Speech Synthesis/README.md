# Speech Synthesis

## 摘要

本篇通过触发浏览器的语音合成API`speechSynthesis`，实现类似google小姐的说话(utterance)功能，包含语音速度(rate)及音频高低(pitch)。

## 重点
 处理流程：

1. 引入API 提供的语言选单。
2. 设定所选语言为说话的语言。
3. 监听rate 及 pitch ，并切换重新说话。 
4. 通过start 及 stop 按钮启动及结束说话。



>01. 引入API 提供的语言选单

- 首先创建语音合成对象实例`let utterance = new SpeechSynthesisUtterance()`。[详见](https://developer.mozilla.org/zh-TW/docs/Web/API/SpeechSynthesisUtterance)

  - `utterance.text` : 说话的文本。
  - `utterance.lang`: 说话的语言。
  - `utterance.pitch`:说话的音频。
  - `utterance.rate`:说话的速度。
  - `utterance.voice`:说话的声音。
  - `utterance.volume`说话的音量。

- 然后利用`speechSynthesis`操作`SpeechSynthesisUtterance()`对象实例。

  方法：

  - `speechSynthesis.speck()`：开始说话。
  - `speechSynthesis.cancel()`：取消说话。
  - `speechSynthesis.pause`：暂停说话。
  - `speechSynthesis.resume()`：暂停状态的重新说话。
  - `speechSynthesis.getVoices()`：获取所有`SpeechSynthesisVoice`声音对象。
    - `SpeechSynthesisVoice`声音对象包含`lang`, `name`等属性。

  监听事件`voiceschanged`：

  `speechSynthesis.onvoicechanged`:语言改变时触发(`speechSynthesis.getVoices`)。

```javascript
  msg.text = document.querySelector('[name="text"]').value;

  function populateVoices() {
    voices = this.getVoices();
    //console.log(voices);
    voicesDropdown.innerHTML = voices.
      map(voice => `<option value="${voice.name}">${voice.name} (${voice.lang})</option>`)
      .join('');
  }

  speechSynthesis.addEventListener('voiceschanged', populateVoices);
```

此时可以看到选取栏内所有语言。

>02. 设定所选语言为说话的语言。

- 定义一个`toggle`方法，切换说话开关。

```javascript
  function setVoice(){
    msg.voice = voices.find(voice => voice.name == this.value);
    toggle();
  }

  function toggle(startOver = true){
    speechSynthesis.cancel();
    if (startOver) {
      speechSynthesis.speak(msg);
    }
  }
  
  voicesDropdown.addEventListener('change', setVoice);
```

此时重选语言会发出语音。

>03. 监听rate 及 pitch的`change`事件，并重新说话。 

`const options = document.querySelectorAll('[type="range"], [name="text"]');`

options内有三个元素，分别为`rate`, `pitch`及`text`，刚好和`SpeechSynthesisUtterance`的属性符合，所以可以监听后传入改变的值，并执行`toggle`。

```javascript
  function setOption() {
    console.log(this.name, this.value);
    msg[this.name] = this.value;
    toggle();
  }
  
  options.forEach(option => option.addEventListener('change', setOption));
```


>04.通过start 及 stop 按钮开启及结束说话。

事件处理函數需要参数时**不能直接传入参数**，如：

`speakButton.addEventListener('click', toggle(false));//x`

 若要传入参数可参考以下作法

```javascript
speakButton.addEventListener('click', ()=>toggle(false)); //用匿名函数
speakButton.addEventListener('click', toggle.bind(null, false));//通过`.bind()`方法
```



```javascript
  speakButton.addEventListener('click', toggle);//开
  stopButton.addEventListener('click', () => toggle(false));//关
```

>05.改进：语言选取栏只显示英文

- 用`.filter()`及`includes('arg')`即可

```javascript
voicesDropdown.innerHTML = voices.
      .filter(voice => voice.lang.includes('en'))
      .map(voice => `<option value="${voice.name}">${voice.name} (${voice.lang})</option>`)
      .join('');
```

> 完整代码

```javascript
<script>
  // 创建SpeechSynthesisUtterance对象
  const msg = new SpeechSynthesisUtterance();
  let voices = [];
  const voicesDropdown = document.querySelector('[name="voice"]');
  const options = document.querySelectorAll('[type="range"], [name="text"]');
  const speakButton = document.querySelector('#speak');
  const stopButton = document.querySelector('#stop');
  //设置text为说话文本
  msg.text = document.querySelector('[name="text"]').value;

  function populateVoices() {
    voices = this.getVoices();
    // console.log(voices);
    voicesDropdown.innerHTML = voices
      .filter(voice => voice.lang.includes('en'))
      .map(voice => `<option value="${voice.name}">${voice.name} (${voice.lang})</option>`)
      .join('');
  }

  function setVoice(){
    msg.voice = voices.find(voice => voice.name == this.value);
    toggle();
  }

  //开关, 预设为true
  function toggle(startOver = true){
    speechSynthesis.cancel();
    if (startOver) {
      speechSynthesis.speak(msg);
    }
  }

  function setOption() {
    console.log(this.name, this.value);
    msg[this.name] = this.value;
    toggle();
  }

  speechSynthesis.addEventListener('voiceschanged', populateVoices);
  voicesDropdown.addEventListener('change', setVoice);
  options.forEach(option => option.addEventListener('change', setOption));
  speakButton.addEventListener('click', toggle);
  stopButton.addEventListener('click', () => toggle(false));

</script>
```



