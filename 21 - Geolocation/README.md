# Geolocation

## 摘要

本篇使用`Navigator.geolocation`的API去获取当前地理位置，包含 经纬度、指向 及 速度，參考[Web API](https://developer.mozilla.org/zh-TW/docs/Web/API/Navigator/geolocation)。

同样需要安全域名(secure origin)，所以也要用`https`或是`localhost`。

建议使用手机模拟器或手机使用此功能。Mac用户可以用[Xcode](https://developer.apple.com/xcode/)模拟。

## 重点

>01.需要先建立一个localhost服务器

npm install, npm run start。

>02.调用`Navigator.geolocation`获取位置。

- `Navigator.geolocation.getPosition()`：单次获取当前地理位置。
- `Navigator.geolocation.watchPosition(data, err)`：连续监测当前地理位置。利用`data.coords.xxx`可以取得数据。第二个参数err是失败时传入`callback`的错误信息。
- `data.coords.xxx`
  - `accurency`"：当前位置的精确度。
  - `latitude`及`longitude`：经纬度。
  - `heading`：当前位置指向。
  - `speed`：当前速度。
- 使用`element.style.transform = rotate()`的方式旋转指针。

```javascript
  const arrow = document.querySelector('.arrow');
  const speed = document.querySelector('.speed-value');

  navigator.geolocation.watchPosition((data) => {
    console.log(data);
    speed.textContent = data.coords.speed;
    arrow.style.transform = `rotate(${data.coords.heading}deg)`;
  }, (err) => {
    console.error(err);
    alert('You should ACCESS the permission!')
  });
```

