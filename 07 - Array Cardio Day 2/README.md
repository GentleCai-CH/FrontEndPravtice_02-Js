# Array Cardio Day2

## 摘要
本篇继Day 4，继续对数组常用方法做介绍。包含`some()`，`every()`，`find()`，`findIndex()`，其中也用到了`splice()`, `slice()`, `...`等。

## 重点
- `Array.prototype.some()`：
	- 只要有一个条件符合返回`true`。`(new Date).getFullYear()`获取当前日期的年份。

```javascript
    const isAdult = people.some(person => ((new Date).getFullYear()) - person.year >=19 );
    console.log(isAdult);
```

- `Array.prototype.every()`：
	- 每一个条件都符合时返回`true`。`

```javascript
    const allAdults = people.every(person => ((new Date).getFullYear()) - person.year >=19 );
    console.log(allAdults);
```

- `find()`:
	- 类似`filter()`，但是`filter`会返回多个符合元素，`find()`只返回一个符合元素。

找到数组索引823423，并返回该元素。

```javacript
    const comment = comments.find(comment => comment.id === 823423);
    console.log(comment);
```

- `findIndex()`:
	- 和`find()`类似，但是只查找`index`。

找到数组索引823423，并返回索引。

```javascript
	const index = comments.find(comment => comment.id === 823423);
```


刪除元素可以使用

- `splice(index, num)`
	- 第一个参数是要刪除的起始索引, 第二个参数是要刪除的数目，第三个参数及后面是要添加的内容。会返回删除后的数组。

- `slice(index, index)`，
	- 第一个参数是起始索引，第二个参数是终点索引（不包含终点），第二个参数若省略表示直到最后。返回一个array object。若是利用拆分的方式，掠过`index`不处理。可以达到`splice()`的效果。
- `...`扩展运算符(ES6功能)
	- 可以实现数组和序列之间的转换。下面的例子因为连续使用`comments.slice()`，会让结果变成`[Array[], Array[]]`，可以使用`...`，把结构变成`[{}, {}, {}, {}, ...]`。

刪除该index元素。

```javascript
	comments.splice(index, 1);
	//or 用slice處理。
	const newComments = [
	    ...comments.slice(0, index),
	    ...comments.slice(index + 1)
    ];
	console.table(comments);
```
