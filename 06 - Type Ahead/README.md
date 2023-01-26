#  AJAX Type Ahead

## 摘要
本篇主要介绍 `Fetch api`用 ajax的方式获取数据(city name)，并根据输入进行搜索，会使用到正则表达式处理字符串。
[fetch api](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

## 重点

> 01.利用fetch获取数据

```javascript
const endpoint = 'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json';
const cities = [];
fetch(endpoint)
  .then(blob => blob.json())
  .then(data => cities.push(...data)); //cities为const，不能直接赋值，需要用Array.protorype.push 添加数据。
```

- `fetch()`api:
	- `fetch()api`是标准的web api，不需要引入即可直接使用，不过因为是新的api，会有浏览器兼容问题，详见[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)。
	- `Fetch API`有很多优点，还可以用于实现`Promise`结构，可以更有效的解析(resolve)传回的。

- `Promise`: 
	- `Promise`是一个`异步(async)操作执行的結果`。`Promise`的初始状态为`等待中(pending)`，执行异步任务后，会返回结果，无论成功(fulfilled)或失败(rejected)都会返回结果，然后状态不再改变，所以每次执行都会返回新的`Promise`对象。

- `Response`:
	- `Promise`被`解析(resolve)`后会返回`Response`对象，可以直接使用`.then()`方法，且能使用`Response`提供的`json()`方法获取数据。 

- async/await:
	- `async`前綴表示异步函数。异步函数内可以使用`await`关键字，当`await`执行时，会暂停该`async function`执行，等待`await function`返回结果后继续执行。返回失败(rejected)则抛出异常

```javascript
async function foo(param){
	var aa = await bar(); //会先暂停foo 的执行，等待await bar()返回结果后才执行console.log(aa);
	
	console.log(aa);
}

```

>02.处理输入文字，获取特定数据。

- `RegExp(this.value, 'gi')`:
	- `g`代表global,`i`代表insensitive,不受大小写影响。 

获取特定数据。

```javascript
function findMatches(wordToMatch, cities){
  return cities.filter(place => {
    const regex = new RegExp(wordToMatch, 'gi');
    return place.city.match(regex) || place.state.match(regex)
  });
}
```


> 03.将符合的数据渲染出来。

- 对输入框设置`keyup`及`change`监听事件，并触发`displayMatehes`函数。

```javascript
function displayMatches(){

}

const searchInput = document.querySelector('.search');
const suggestions = document.querySelector('.suggestions');

searchInput.addEventListener('change', displayMatches);
searchInput.addEventListener('keyup', displayMatches);
```

> 04.定义处理函数`displayMatches`。

- 将获取的数据重新`map`，并用css方式显示出来。
- `join()`:
	- 把`arrayObject`中的所有元素放入字符串中。
- 返回 `html`：
	- 使用反引号` `` `包裹内容，用`${value}`传递参数。

```javascript
function displayMatches(){
  const matchArray = findMatches(this.value, cities);
  const html = matchArray.map(place => {
      const regex = new RegExp(this.value, 'gi');
      const cityName = place.city.replace(regex, `<span class="hl">${this.value}</span>`);
      const stateName = place.state.replace(regex, `<span class="hl">${this.value}</span>`);
      return `
		<li>
		<span class="name">${cityName}, ${stateName}</span>
		<span class="population">${place.population}</span>
		</li>
	`;
  }).join('');
  suggestions.innerHTML = html;
}
```

> 05.处理正则表达式

```javascript
function numberWithCommas(x) {
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}

function displayMatches(){
  ...
  <span class="population">${numberWithCommas(place.population)}</span>
}
```

