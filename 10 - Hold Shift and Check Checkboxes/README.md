# 10 Hold Shift and Check Checkboxes

## 摘要

本篇主要是实现通过按下shift鍵和`click`选取多个checkbox的功能。



## 重点

>01. 首先对checkbox定义事件监听

```javascript
const checkboxes = document.querySelectorAll('inbox input[type="checkbox"]');

checkboxes.forEach(chechbox.addEventListener('click' , handleCheck));
```


>02.定义处理函数handleCheck 方法。

- 思路：
  - `shift`是在点击`checkbox`后再按下，且可以由`shiftKey`判定是否按下，当两者满足时，再点击另一个`checkbox`出现效果；
  - 可以先定义最后点击的是`lastChecked`，用`inBetween`表示两个`checkbox`之间的内容，若在两个之间则为`true`, 否则为`false`。
  - 所以最后只要判断`inBetween`是否为`true`进行打勾即可。

```javascript
let lastChecked;

function handleChecj(e){
  let inBetween = false;
  if (e.shiftKey || this.checked){
    checkboxes.forEach(checkbox => {
      if (checkbox === this || checkbox === lastChecked){
        inBetween = !inBetween;
      }
      if (inBetween){
        checkbox.checked = true;
      }
    });
  }
  lastChecked = this
}
```

