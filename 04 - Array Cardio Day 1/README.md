# Array Cardio Day1

## 摘要
本篇主要介绍几个JS array的方法。包含`filter()`，`map()`，`sort()`，`reduce()`。

## 重点
- `filter()`:  用來过滤数组中的元素，并返回结果。

```Javascript
const fifteen = inventors.filter(inventor => (inventor.year >= 1500 && inventor.year < 1600));
console.table(fifteen)//会以table的形式显示。
```

- `map()`:按条件组合数组中的元素，并返回结果。

```Javascript
const fullNames = inventors.map(inventor => inventor.first+ '' + inventor.last);
```

- `sort()` :排序，比较a.b  若`a< b`，a排在b前面，则返回小于0的值(一般为 -1)，若`a>b`，a在b后面，则返回大于0的值(1)

```javascript
const ordered = inventors.sort((a, b) => a.year > b.year ? 1 : -1); //排序出生日期

const oldest = inventors.sort((a, b) => { //排序年齡，由大到小
	const lastGuy = a.passed - a.year;
	const nextGuy = b.passed - b.year;
    return lastGuy > nextGuy  ? -1 : 1 ;
})
```

- `reduce()` :  常用于累加，第一个arg为回调函数，第二个为数组的 起始值或返回形式。回调函数的第一个arg为结果，第二个为数组内的元素。

```javascript
const totalYears = inventors.reduce((total, inventor) => { 
	return total + (inventor.passed - inventor.year)
}, 0);
```

- `map()` + `filter`应用:`querySelectorAll('')`取得的值是NodeList，需要用`Array.from()`将其转为数组，才可以使用数组方法。选取网页中含有de的的街道，使用`.includes('de')`

```javascript
const category = document.querySelector('.mw-category');
const links = Array.from(category.querySelectorAll('a'));
const de = links
	.map(link => link.textcontent) //取得链接的文字
        .filter(streetName => streetName.includes('de')); //过滤出 符合元素
```

- `sort()`练习:使用到`split()`方法，拆分string成array。

```javascript
const alpha = people.sort((lastOne, nextOne) => {
	const [aLast, aFirst] = lsatOne.split(', ');
    const [bLast, bFirst] = nextOne.split(', ');
    return aLast > bLast ? 1:-1;
})
```

- `reduce()` 练习，计算数组内重复元素个数。若数组内index没有该项，则新增为0，有则累加。reduce的第二个参数为`{}`，表示返回一个对象。

```javascript
const transportation = data.reduce((obj, item)=>{
	if(!obj[item]){
    	obj[item] = 0;
    }
    obj[item]++;
    return obj;
}, {});
```
