# Sort Without Articles

# 摘要

本篇通过编排文章的排序，复习`sort`, `map`, `join` 及`replace`, `trim`等方法的使用。

## 重点

>01.编排文章

- Array.prototype`.sort(function (a, b){})`: 放入a, b，若a>b， 表示a要往前排（降序）则回传+1, 否则表示要往后排（升序）则回传-1。
- Array.prototype`.map(funciton(name) => ...)`:遍历数组的每一个元素。
- Array.prototype`.join('')`:合并数组的所有元素。若直接用`toString()`会留下逗号。

```javascript
const sortedBands = bands.sort((a, b) => a > b ? 1: -1);

document.querySelector('#bands').innerHTML = 
  sortedBands.map(band => `<li>${band}</li>`)
  .join('');
```

>02.排序：不考虑a, an 及the

- 利用正则表达式进行排序，为了避免改变数组内容，仅在比较时排序。
- `String.prototype.replace`:字符串内容，通常会使用正则表达式。
- `String.prototype.trim()`:消除字符串左右`空白`。

```javascript
function strip(bandName){
  return bandName.replace(/^(a |an |the)/i, '').trim();
}
...strip(a)> strip(b)? 1 : -1;
```

>03.完整代码

```javascript
<script>
const bands = ['The Plot in You', 'The Devil Wears Prada', 'Pierce the Veil', 'Norma Jean', 'The Bled', 'Say Anything', 'The Midway State', 'We Came as Romans', 'Counterparts', 'Oh, Sleeper', 'A Skylit Drive', 'Anywhere But Here', 'An Old Dog'];

function strip(bandName) {
  return bandName.replace(/^(a |the |an )/i, '').trim();
}

const sortedBands = bands.sort((a, b) => strip(a) > strip(b) ? 1: -1);

document.querySelector('#bands').innerHTML =
  sortedBands.map(band => `<li>${band}</li>`)
  .join('');

</script>
```

