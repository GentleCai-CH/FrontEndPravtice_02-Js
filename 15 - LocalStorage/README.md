# LocalStorage and Event Delegate

## 摘要

本篇主要介绍两部分:

- `LocalStorage`的使用。
- `Event Delegate`事件代理

## 重点

>01.LocalStorage

- 浏览器三种缓存方式：`Cookie`, `LocalStorage`及`SessionStorage`。可参考三者差异[Cookie vs LocalStorage vs SessionStorage](http://jerryzou.com/posts/cookie-and-web-storage/)

  | 特性          | Cookie                                   | LocalStorage               | SessionStorage             |
  | ----------- | ---------------------------------------- | -------------------------- | -------------------------- |
  | 生命周期        | 由Server端生成，可设置失效时间。若是browser生成，默认browser关闭后失效 | 除非被清除，否则永久保存               | 当前有效，关闭页面或浏览器刪除。           |
  | 大小          | 4KB                                      | 5MB                        | 5MB                        |
  | 与server通信状况 | 每次发送HTTP请求时皆会存在Header中。                   | 不参与Server通信                | 不参与Server通信                |
  | 易用性         | 需自行封裝，原生API难处理                          | 原生API难处理，可再封装成object及array | 原生API难处理，可再封装成object及array |
  | 应用场景        | 用户登录                                 | 可将购物车信息cookie传送过来。         | 表单页面处理。                    |

>02.举例：添加item

- `e.preventDefault()`:取消默认的触发效果，如取消`submit`的默认提交效果。
- 当对象的`key` 和 `value`相同时，可以使用ES6的方法，只写单个名称即可。

```javascript
  function addItem(e){
    e.preventDefault();
    const text = (this.querySelector('[name=item]')).value;
    const item = {
      text,
      done:false
    }
    item.push(item);
  }

  addItems.addEventListener('submit', addItem);
```

可以在`console`内输入`items`检查。

>03.将items以列表形输出

- `populateList`处理添加的item。
- 用`.map()`处理每一项的输出，利用`join('')`，将数组转换成连续字符串。
- `this.reset()`:用來清空输入栏位。

```javascript
function addItem(e){
    ...
    items.push(item);
    populateList(items, itemsList);
    this.reset();
}

function populateList(plates = [], platesList){
  platesList.innerHTML = plates.map((plate, i)=>{
    return `
    	<li>
    		<input type="checkbox" data-index=${i} id="item${1}" ${plate.done ? 'checked' : ''}/> //若done为true则显示checked，若无则显示空字符串。
    		<label for="item${i}">${plate.text}</label>
		</li>
    `;
  }).join('');
}
```

>04.使用LocalStorage

- `LocalStorage.setItem('name', 'value')`:存入。
- `LocalStorage.getItem('name', 'value')`:获取。
- 直接输入数组时，LocalStorage会存入`{Object}`，所以需要使用`JSON.stringity`把对象转换为`string`存储，之后使用`JSON.parse()`将其转换为对象。

```javascript
const items = JSON.parse(localStorage.getItem('items')) || []; //修改items读取方式
function addItem(e){
  ...
  populateList(items, itemsList);
  localStorage.setItem('items', JSON.stringify(items));
}
...
 populateList(items, itemsList);//需要在最后插入这段，否则每次re-load之后还要按下submit才会出现!
```

目前可以看到item显示!也可以检查浏览器的LocalStorage是否有存值。

>
>05.Event Delegate事件代理

- `Event Delegate`:一般使用`event binding`绑定特定DOM元素后注册Event监听器并触发DOM function即可。而Event Delegate则是绑定DOM的祖父元素，让子元素通过父元素触发处理函数。
- 
- 例子：先用`Event binding`绑定子元素后新增元素，新增的元素没有注册监听。此时通过`Event Delegate`绑定父元素，子元素事件冒泡后传给父元素，也会被父元素绑定的事件触发。
- 
- `toggleDone()`用`click`事件绑定`itemsList`(.plates)，子元素('input')被`click`后触发toggleDone，打印`e.target`会发现返回`input`及`label`两个元素。此时筛选`if (!e.target.matches('input')) return;`然后把点击的item储存起来切换`done`的值，并存入`LocalStorage`。

```javascript
  function toggleDone(e) {
    if (!e.target.matches('input')) return; // skip this unless it's an input
    const el = e.target;
    const index = el.dataset.index;
    items[index].done = !items[index].done;
    localStorage.setItem('items', JSON.stringify(items));
    populateList(items, itemsList);
  }
```
