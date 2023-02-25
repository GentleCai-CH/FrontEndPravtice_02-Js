# WebCam Fun

## 摘要

本篇使用原生的Javascript触发电脑摄像头`webcam`，并输出到`canvas`上，用`canvas`进行拍照及特效处理。

## 重点
>
>01.需要先建立一个localhost服务器。

- 要调用的`getUserMedia()`api需要在`安全域名(secure origin)`下使用。即包含`HTTPS`, `Localhost`等。[可參考文件](https://www.chromium.org/Home/chromium-security/prefer-secure-origins-for-powerful-new-features)

- `browser-sync`，可以在本地架设server的插件。用node.js的`npm`来安装`browser-sync`
- 
- - 若还没有安裝node.js，先下载安装[node.js](https://nodejs.org/en/)。
- `npm`的相关指令，可以在[node.js](https://nodejs.org/en/)上查找，`npm`执行的内容主要是依照`package.json`的脚本，如文件夹内的`package.json`。
- 
- `devDependencies`将要下载的依赖包，会处理相关的依赖关系。
- `scripts`是可执行的指令。用法:`npm run {scripts}`，如`npm run start`。


-在命令行工具执行以下命令

```javascript
npm install //安装package.json内的依赖包
npm run start //开启browser-sync 插件，按ctrl+右键连接。
```

>02.定义变量

- `video`:原始镜头的位置
- `canvas`:画布，呈现截图和特效的位置。
- `strip`:产生图像的位置。
- `snap`:点击照相时产生的音效(click)。

>03.获取webcam设备。

- 用`navigator.mediaDevices.getUserMedia()`获取摄像头权限。会返回一个`promise`對象。
- 
- 若是选择允许，则传回`resolved`状态，resolved信息为`MediaStream`对象。若是选择拒绝(deny)，则传回`Reject`状态，调用`PermissionDeniedError`回調。
- 
- `window.URL.createObjectURL(localMediaStream)`：创建一个带有URL的`DOMString`，表示传入参数的对象。该URL的生命周期与创建它的window一致。新的对象表示指定的`File`对象或`Blob`对象

```javascript
function getVideo(){
	navigator.mediaDevices.getUserMedia({ video : true, audio : false})
		.then(localMediaStream => {
			console.log(localMediaStream);
			video.src = window.URL.createObjectURL(localMediaStream);
			video.play();
		}).catch(err => {
			console.log(`You must deny the webcam permission`, err);
		});
}

getVideo();
```

此时可以看到权限请求，点击允许后可以看到摄像头。

>04.将截图放到Canvas画布上

- 定义画布尺寸。
- `setInterval(function(){}, ms)`:设置运行频率。单位为毫秒。
- `ctx.drawImage(img, x, y, width, heigth)`：获取截图。
- `ctx.getImageData(x, y, width, height)`：返回ImageData对象，表示canvas区域内的像素数据。
- `ctx.putImageData(img, x, y)`：将ImageData对象的数据绘制到图像。
- 绑定事件`canplay`：摄像头开始使用时触发(已完成buffer程序可随时开始)。

```javascript
function paintToCanvas(){
	const width = video.videoWidth;
	const height = video.videoHeight;
	canvas.width = width;
	canvas.height = height;

	return setInterval(() => {
		ctx.drawImage(video, 0, 0, width, height);
		
		let pixels = ctx.getImageData(0, 0, width, height);

		ctx.putImageData(pixels, 0, 0);
	}, 16);
}
getVideo();
video.addEventListener('canplay', paintToCanvas);
```

>05.添加滤镜
- 在获取图像`ctx.getImageData`及放回图像`ctx.putImageData(pixels, 0, 0);`过程中，可以对`ImageData`定制滤镜效果。
- ImageData包含了大量的数据，其中包括rbga值(red, blue, green, alpha)。

>06.拍照功能

拍照分两部分处理:

1. 按下后产生声音

```javascript
function takePhoto(){
	snap.currentTime = 0;
  	snap.play();   
}
```

2. 获取截图

- `canvas.toDataURL('image/jpeg')`:返回URL, type可在参数内自定。[参考](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLCanvasElement/toDataURL)
- 创建一个a元素，并设置href值，插入文档中，可以看到截图效果。
- 若想要设置下载功能:`link.setAttribute('download', 'fileName');`
- `strip.insertBefore(data, location)`：插入节点。
  - location使用:`firstChild`, `lastChild`, `childNode[key]`。

```javascript
function takePhoto() {
  // 音效
  snap.currentTime = 0;
  snap.play();

  // 从画布取出数据
  const data = canvas.toDataURL('image/jpeg');
  const link = document.createElement('a');
  link.href = data;
  link.setAttribute('download', 'handsome');
  link.innerHTML = `<img src="${data}" alt="Handsome Man" />`;
  strip.insertBefore(link, strip.firsChild);
}
```


>07.特效功能。

其实只是处理RGB的像素，输入的`pixels.data[]`数组内分別以`red(data[i]), green(data[i+1]), blue(data[i+2]), alpha(data[i+3])`排列。故`i+=4`。

##### 1. redEffect()

 - 把red的项目加深处理，蓝绿的项目分别-50和减半。

```javascript
function redEffect(pixels) {
  for(let i = 0; i < pixels.data.length; i+=4) {
    pixels.data[i + 0] = pixels.data[i + 0] + 200; // RED
    pixels.data[i + 1] = pixels.data[i + 1] - 50; // GREEN
    pixels.data[i + 2] = pixels.data[i + 2] * 0.5; // Blue
  }
  return pixels;
}
```

在`paintToCanvas()`内获取`ImageData`加入。

```
return setInterval(() => {
		ctx.drawImage(video, 0, 0, width, height);
		//取出像素
		let pixels = ctx.getImageData(0, 0, width, height);

		//特效
		 pixels = redEffect(pixels);

		//取回
		ctx.putImageData(pixels, 0, 0);
	}, 16);
```

##### 2. rgbSplit()

- 将rgb像素分离。

```javascript
function rgbSplit(pixels) {
  for(let i = 0; i < pixels.data.length; i+=4) {
    pixels.data[i - 150] = pixels.data[i + 0]; // RED
    pixels.data[i + 500] = pixels.data[i + 1]; // GREEN
    pixels.data[i - 550] = pixels.data[i + 2]; // Blue
  }
  return pixels;
}
```

同样在`paintToCanvas()`内获取`ImageData`加入。


##### 3. greenScreen()

- 最后是greenScreen，先取消html内注释部分。可以看到六個`range` bar。分別代表rgb的数值。

```javascript
function greenScreen(pixels) {
  const levels = {};

  document.querySelectorAll('.rgb input').forEach((input) => {
    levels[input.name] = input.value;
  });

  for (i = 0; i < pixels.data.length; i = i + 4) {
    red = pixels.data[i + 0];
    green = pixels.data[i + 1];
    blue = pixels.data[i + 2];
    alpha = pixels.data[i + 3];

    if (red >= levels.rmin
      && green >= levels.gmin
      && blue >= levels.bmin) {
      // 取出
      pixels.data[i + 3] = 0;
    }
  }
  return pixels;
}
```

> 
>
>完整代码

```javascript
const video = document.querySelector('.player');
const canvas = document.querySelector('.photo');
const ctx = canvas.getContext('2d');
const strip = document.querySelector('.strip');
const snap = document.querySelector('.snap');

function getVideo(){
	navigator.mediaDevices.getUserMedia({ video : true, audio : false})
		.then(localMediaStream => {
			console.log(localMediaStream);
			video.src = window.URL.createObjectURL(localMediaStream);
			video.play();
		}).catch(err => {
			console.log(`You must deny the webcam permission`, err);
		});
}

function paintToCanvas(){
	const width = video.videoWidth;
	const height = video.videoHeight;
	canvas.width = width;
	canvas.height = height;

	return setInterval(() => {
		ctx.drawImage(video, 0, 0, width, height);
		//take the pixels out
		let pixels = ctx.getImageData(0, 0, width, height);

		// mess with them
		// pixels = redEffect(pixels);

		// pixels = rgbSplit(pixels);

		pixels = greenScreen(pixels);
		//put them back
		ctx.putImageData(pixels, 0, 0);
	}, 16);
}

function takePhoto() {
  // played the sound
  snap.currentTime = 0;
  snap.play();

  // take the data out of the canvas
  const data = canvas.toDataURL('image/jpeg');
  // console.log(data);
  const link = document.createElement('a');
  link.href = data;
  link.setAttribute('download', 'handsome');
  link.innerHTML = `<img src="${data}" alt="Handsome Man" />`;
  strip.insertBefore(link, strip.firsChild);
}

function redEffect(pixels) {
  for(let i = 0; i < pixels.data.length; i+=4) {
    pixels.data[i + 0] = pixels.data[i + 0] + 200; // RED
    pixels.data[i + 1] = pixels.data[i + 1] - 50; // GREEN
    pixels.data[i + 2] = pixels.data[i + 2] * 0.5; // Blue
  }
  return pixels;
}

function rgbSplit(pixels) {
  for(let i = 0; i < pixels.data.length; i+=4) {
    pixels.data[i - 150] = pixels.data[i + 0]; // RED
    pixels.data[i + 500] = pixels.data[i + 1]; // GREEN
    pixels.data[i - 550] = pixels.data[i + 2]; // Blue
  }
  return pixels;
}

function greenScreen(pixels) {
  const levels = {};

  document.querySelectorAll('.rgb input').forEach((input) => {
    levels[input.name] = input.value;
  });

  for (i = 0; i < pixels.data.length; i = i + 4) {
    red = pixels.data[i + 0];
    green = pixels.data[i + 1];
    blue = pixels.data[i + 2];
    alpha = pixels.data[i + 3];

    if (red >= levels.rmin
      && green >= levels.gmin
      && blue >= levels.bmin) {
      // take it out!
      pixels.data[i + 3] = 0;
    }
  }

  return pixels;
}

getVideo();

video.addEventListener('canplay', paintToCanvas);
```

