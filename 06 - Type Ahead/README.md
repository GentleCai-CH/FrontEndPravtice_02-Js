#  AJAX Type Ahead

## 摘要
本篇主要介绍 `Fetch api`用 ajax的方式获取数据(city name)，并根据输入进行搜索，会使用到正则表达式处理字符串。
[fetch api](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

## 重点

> 利用fetch获取数据

```javascript
const endpoint = 'https://gist.githubusercontent.com/Miserlou/c5cd8364bf9b2420bb29/raw/2bf258763cdddd704f8ffd3ea9a3e81d25e2c6f6/cities.json';
const cities = [];
fetch(endpoint)
  .then(blob => blob.json())
  .then(data => cities.push(...data)); //cities为const，不能直接赋值，需要用Array.protorype.push 添加数据。
```

- `fetch()api`:
	- `fetch()api`是标准的web api，不需要引入即可直接使用，不过因为是新的api，会有浏览器兼容问题，详见[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)。
	- `Fetch API`有很多优点，还可以用于实现`Promise`结构，可以更有效的解析(resolve)传回的。

- `Promise`: 
	- `Promise`是一个`异步(async)操作执行的結果`。`Promise`的初始状态为`等待中(pending)`，执行异步任务后，会返回结果，无论成功(fulfilled)或失败(rejected)都会返回结果，然后状态不再改变，所以每次执行都会返回新的`Promise`对象。

- `Response`:
	- `Promise`被`解析(resolve)`后会返回`Response`对象，可以直接使用`.then()`方法，且能使用`Response`提供的`json()`方法获取数据。 

##### 补充 async/await
- `async/await`:
	- `async`前綴表示异步函数。异步函数内可以使用`await`关键字，当`await`执行时，会暂停该`async function`执行，等待`await function`返回结果后继续执行。返回失败(rejected)则抛出异常

```javascript
async function foo(param){
	var aa = await bar(); //会先暂停foo 的执行，等待await bar()返回结果后才执行console.log(aa);
	
	console.log(aa);
}

```

>处理输入文字，获取特定数据。

- `RegExp()`:用來做正規表達試的參數， `g`代表global,`i`代表insensitive,不受大小寫影響。 

```javascript
function findMatches(wordToMatch, cities){
  return cities.filter(place => {
    const regex = new RegExp(wordToMatch, 'gi');
    return place.city.match(regex) || place.state.match(regex)
  });
}
```

到這邊就確定可以透過`findMatches()`取到特定的資料了。

> 接下來要把資料依照查找的字串render出來。

- 使先對輸入的自框建立`keyup`及`change`監聽事件，並觸發`displayMatehes`的查找事件。

```javascript
function displayMatches(){

}

const searchInput = document.querySelector('.search');
const suggestions = document.querySelector('.suggestions');

searchInput.addEventListener('change', displayMatches);
searchInput.addEventListener('keyup', displayMatches);
```

> 定義查找事件`displayMatches`的內容。

- 首先先取得資料，並把資料重新`map`，並把查詢的字用css方式顯示出來。
- `join()`:把`arrayObject`中的所有元素放入一個字串中。
- 如果是要return `html`的語法要用` `` `包裹，中間要傳遞的參數可以用`${value}`，帶入。

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

> 最後再處理人數的顯示處理問題。

- 用`numberWithCommas`處理正規表達試的問題。

```javascript
function numberWithCommas(x) {
  return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
}

function displayMatches(){
  ...
  <span class="population">${numberWithCommas(place.population)}</span>
}
```

> 今日項目完成！

