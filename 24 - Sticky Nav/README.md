# Sticky Nav

## 摘要

本篇实现当scroll超过Nav时，将Nav 固定在上方的效果。

##重点

流程：

1. 比较数值：scrollY vs topOfNav
2. 修改Css效果。
3. 针对jerky jump进行处理。
4. 加入css动画效果修正。

> 01. 比较数值：scrollY vs topOfNav


- `window.scrollY`:已經scroll的距离。
- `element.offsetTop`:元素顶部的距離。

```javascript
  const nav = document.querySelector('#main'); //获取nav
  const topOfNav = nav.offsetTop; // 设置nav到window的距离

  function fixNav(){
    console.log(topOfNav, window.scrollY);
  }
  window.addEventListener('scroll', fixNav); //绑定事件scroll
```

可以可以观察`topOfNav`及`window.scrollY`的变化。

`topOfNav`会固定不变，`window.scrollY` 随scroll变化。


>02. 修改Css效果。

修改`fixNav function`，若高度超过，则在`body`新增`fixed-nav` 的class，反之移除。

```javascript
  function fixNav(){
    // console.log(topOfNav, window.scrollY);
    if (window.scrollY >=  topOfNav){
      document.body.classList.add('fixed-nav');
    }else{
      document.body.classList.remove('fixed-nav');
    }
  }
```

```css
body.fixed-nav nav {
  position: fixed;
  box-shadow: 0 5px 0 rgba(0,0,0,0.1);
}
```


>03. 针对jerky jump进行处理。


当nav被固定时会出现一个问题，因为是对nav 下fixed ，所以nav会脱离原本的文档，造成下方的元素往上走(jerky jump)，所以需要在文档内加入空白，让不要往上跳跃。

```javascript元素
  function fixNav(){
    // console.log(topOfNav, window.scrollY);
    if (window.scrollY >=  topOfNav){
      document.body.style.paddingTop = nav.offsetHeight + 'px';
      document.body.classList.add('fixed-nav');
    }else{
      document.body.classList.remove('fixed-nav');
      document.body.style.paddingTop = 0;
    }
  }
```

跳动消失(被padding取代)。


>04. 加入css动画效果修正。

```css
// fixed-nav时 logo跳出，在li.logo内定义了动画的时间(transition:all 0.2s)
.fixed-nav li.logo { 
  max-width: 500px;
}
//fixed-nav显示时，放大文本內容，從0.98->1
body.fixed-nav .site-wrap {
  transform: scale(1);
}
```

>完整代码

```javascript
<script>
  const nav = document.querySelector('#main');
  const topOfNav = nav.offsetTop;

  function fixNav(){
    // console.log(topOfNav, window.scrollY);
    if (window.scrollY >=  topOfNav){
      document.body.style.paddingTop = nav.offsetHeight + 'px';
      document.body.classList.add('fixed-nav');
    }else{
      document.body.classList.remove('fixed-nav');
      document.body.style.paddingTop = 0;
    }
  }

  window.addEventListener('scroll', fixNav);

</script>
```

```css
body.fixed-nav .site-wrap {
  transform: scale(1);
}

body.fixed-nav nav {
  position: fixed;
  box-shadow: 0 5px 0 rgba(0,0,0,0.1);
}

body.fixed-nav li.logo {
  max-width: 500px;
}
```

