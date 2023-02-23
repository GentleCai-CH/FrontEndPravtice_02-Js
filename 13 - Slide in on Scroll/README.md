# Slide in on Scroll

## 摘要

本篇实现图片的移入移出效果。当窗口出现图片区域时以动画方式出现。

## 重点

>01.获取元素，设置监听。

- 移动右方scroll bar时触发`scroll`事件，一般是使用`window.addEventListener()`，但是该事件类型触发频繁，需要加时间限制。
- 这里使用的是`lodash`的方法`debounce`，并将监听设置为20minisec

```javascript
const sliderImages = document.querySelectorAll('.slide-in');
function checkslide(){
  sliderImages.forEach(sliderImage => {
    
  })
}

window.addEventListener('scroll', debounce(checkSlide));
```

>02.获取图片位置。

- `window.scroll`:滑动时顶部。
- `window.innerHeight`:窗口高度。故`window.scroll+window.innerHeight`等于滑动时的底部。
- `sliderImage.height`：图片高度。
- `slideInAt=window.scroll+window.innerHeight - sliderImage.height/2`表示滑动到图片的一半高度。
- `sliderImage.offsetTop`:表示图片顶部在文档的高度。
- `imageBottom = sliderImage.offsetTop + sliderImage.height;`:表示图片底部在文档的高度。
- `isHalfShown = slideInAt > sliderImage.offsetTop;`:当窗口移动超过图片高度时显示。
- `const isNotScrolledPast = window.scrollY < imageBottom;`:窗口移动时，图片的底部高度未到窗口顶部时。
- 
- 当满足`isHalfShown&&isNotScrolledPast`时，表示窗口移动超过图片一半高度，且图片的底部高度还未超过窗口顶部，添加动画样式class，不满足两个条件时则移除动画。

```javascript
function checkSlide() {
      sliderImages.forEach(sliderImage => {
        // half way through the image
        const slideInAt = (window.scrollY + window.innerHeight) - sliderImage.height / 2;
        // bottom of the image
        const imageBottom = sliderImage.offsetTop + sliderImage.height;
        const isHalfShown = slideInAt > sliderImage.offsetTop;
        const isNotScrolledPast = window.scrollY < imageBottom;
        if (isHalfShown && isNotScrolledPast) {
          sliderImage.classList.add('active');
        } else {
          sliderImage.classList.remove('active');
        }
      });
    }
```
