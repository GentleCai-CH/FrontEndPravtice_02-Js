# Adding Up Time With Reduce

## 摘要

本篇通过计算多个video的时长总和，复习`reduce`及`map`方法。

## 重点

>01.用`.map()`方法选取时间后，再利用`.map()`方法把时间计算成秒，接着用`.reduce()`把全部加起来，并切分成时:分:秒。

- 使用`document.querySelectorAll('')`时，获取到的是`NodeList`，需要使用`forEach()`。
- 
- 把`NodeList`转换成`Array`的方法有2:
  - 使用扩展运算符：`...document.querySelectorAll('')`:
  - `Array.from(document.querySelectorAll())`
- 
- `[].map(function (x){return parseFloat(x)})`可简化为`[].map(parseFloat)`
- `Array.prototype.reduce(arg1, arg2)`:`arg1`是要加和的结果，`arg2`是要加和的对象。
- 
- `Math.floor()`:向下取整。
- `String.prototype.split('arg')`：按照参数切割字符串。
- `Number.parseFloat`:把字符串(String)转换成数值(Int)类型。


```javascript
  const timeNodes = Array.from(document.querySelectorAll('[data-time]'));
  const seconds = timeNodes
                  .map(node => node.dataset.time) //取得时间
                  .map(timeCode => { //分割时间6:30, 转换成数值后加和。
                    const [mins, secs] = timeCode.split(':').map(parseFloat);
                    return mins * 60 + secs;
                  }).reduce((total, vidSeconds) => total + vidSeconds);
  let secondsLeft = seconds; //拆解成 时:分:秒

  const hours = Math.floor(secondsLeft/3600);//向下取整
  secondsLeft = secondsLeft % 3600; //取余

  const mins = Math.floor(secondsLeft/60);
  secondsLeft = secondsLeft % 60;

  console.log(hours, mins, secondsLeft);
```
