# 25 - Event Capture, Propagation, Bubbling and Once

## 摘要

本篇介绍DOM事件的机制:Event Capture(事件捕捉), Event Bubbling(事件冒泡), Propagation, 及Once(单次执行)。

## 重点

>01.注册监听
```html
  <div class="one">
    <div class="two">
      <div class="three">
      </div>
    </div>
  </div>
```

这里有3层div。首先都加入`click`监听。

```javascript
  const divs = document.querySelectorAll('div');

  function logText(e){
    console.log(this.classList.value);
  }

  divs.forEach(div => div.addEventListener('click', logText));
```

元素的监听注册`.addEventListener()`有三个参数。

`EventTarget.addEventListener('eventName', callback, option)`:分別为事件类型，处理回调，及选项(option)。

option内有两个属性`capture`及`once`默认为`false`。

```javascript
divs.forEach(div => div.addEventListener('click', logText, {
  capture : false,
  once : false,
}));
```

点击最里面的`div`，会依次出现three, two, one。

>02.捕获和冒泡
- 注册一个监听器时，会**由外向内**[捕捉](Capture)监听器的位置，
- 而当位置被触发时，会**由内向外**［冒泡］(bubbling)。
- `capture`默认为`false`，表示不处理［捕捉过程]，只处理［冒泡过程］。

```javascript
  const divs = document.querySelectorAll('div');

  function logText(e){
    console.log(this.classList.value); //three, two, one
  }

  divs.forEach(div => div.addEventListener('click', logText, {
  capture : false,
  once : false,
}));
```



将`capture`设为true时，则不会执行［冒泡过程］，所以会返回`one->two->three`。

```javascript
  const divs = document.querySelectorAll('div');

  function logText(e){
    console.log(this.classList.value); //one, two, three
  }

  divs.forEach(div => div.addEventListener('click', logText, {
  capture : true,
  once : false,
}));
```



而加入了`e.stopPropagation();`时，会中断过程。若`capture`为`ture`, 则只会返回one；若为`false`，则只会返回`three`。

```javascript
  const divs = document.querySelectorAll('div');

  function logText(e){
    console.log(this.classList.value); 
    //capture:true->one, capture:false->three
    e.stopPropagation();
  }

  divs.forEach(div => div.addEventListener('click', logText, {
  capture : true,
  once : false,
}));
```

而`once`表示监听器只执行一次监听事件，执行完后`removeEventListener`。

```javascript
  button.addEventListener('click', () => {
    console.log('Click!!!');
  }, {
    once: true
  });
```

点击按钮后只会出现一次`Click!!!`。

>完整代码

```javascript
  const divs = document.querySelectorAll('div');
  const button = document.querySelector('button');

  function logText(e) {
    console.log(this.classList.value);
    // e.stopPropagation(); // stop bubbling/capture!
  }

  divs.forEach(div => div.addEventListener('click', logText, {
    capture: false,
    once: true
  }));

  button.addEventListener('click', () => {
    console.log('Click!!!');
  }, {
    once: true
  });	
```







