# Speech Detection

## 摘要

本篇使用原生的**语音识别API**`web speech api`。同样需要创建本地服务器和同意麦克风使用权限。

## 重点

>01.需要先建立一个localhost服务器

`npm install`, `npm run start`。

>02.新建语音识别对象，并赋值。

- `window.SpeechRecognition`：触发语音识别API。
- `window.webkitSpeechRecognition`：Firefox浏览器语音识别API。
- `recognition.interimResults = true`:控制语音识别过程中是否返回，若为`true`则一直返回，若`SpeechRecognitionResult.isFinal`为`true`时，表示当前对话结束。
- `recognition.lang = 'en-US';`设置识别语言。繁体中文:`zh-tw`，简体中文：。

```javascript
  window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

  const recognition = new SpeechRecognition();
  recognition.interimResult = true;
  recognition.lang = 'en-US';
```

>03.添加元素到文档上，监听`result`事件。

- `.appendChild()`:添加元素。
- `recognition.start()`:开始启动识别。

```javascript
  let p = document.createElement('p');
  const words = document.querySelector('.words');
  words.appendChild(p);

  recognition.addEventListener('result', e =>{
    console.log(e);
  });

  recognition.start();
```

此时对着麦克风说话可以在`console`看到回传的事件e：SpeechRecognitionEvent`。

打开事件后，`results[0][transcript]`内可以看到转换的内容。

>04.处理回传的事件e

- 获取`results`的内容。重新排列成字符串。
- 语音暂停，另起一个段落。语音可以用`e.result[0].isFianl=true`判断。
- 暂停后重启：须增加监听`  recognition.addEventListener('end', recognition.start);`，并重新启动。

```javascript
  recognition.addEventListener('result', e =>{
    const transcript = Array.from(e,results)
                        .map(result => result[0])
                        .map(result => result.transcript)
                        .join('');

    if (e.results[0].isFinal) {
        p = document.createElement('p');
        words.appendChild(p);
      }
  });

  recognition.addEventListener('end', recognition.start);
```

>05.增加功能
- 提到`poop, poo, shit, dump`自动转换成其他字符


```javascript
      const poopScript = transcript.replace(/poop|poo|shit|dump/gi, '💩');
      p.textContent = poopScript;
```

> 完整代码

```javascript
<script>
  window.SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

  const recognition = new SpeechRecognition();
  recognition.interimResult = true;
  recognition.lang = 'en-US';

  let p = document.createElement('p');
  const words = document.querySelector('.words');
  words.appendChild(p);

  recognition.addEventListener('result', e =>{
    const transcript = Array.from(e,results)
                        .map(result => result[0])
                        .map(result => result.transcript)
                        .join('');

    const poopScript = transcript.replace(/poop|poo|shit|dump/gi, '💩');
    p.textContent = poopScript;

    if (e.results[0].isFinal) {
        p = document.createElement('p');
        words.appendChild(p);
      }
  });

  recognition.addEventListener('end', recognition.start);

  recognition.start();

</script>
```

