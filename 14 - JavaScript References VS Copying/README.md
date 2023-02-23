# Javascript Reference VS copy

## 摘要

本篇介绍Javascript赋值时，何时是用copy，何时是用reference。

## 重点

>01.对于变量，`integer`, `String`及`boolen`都是**copy**。

```javascript
let age = 100;
let age2 = age;
console.log(age, age2); // 100, 100
age = 200
console.log(age, age2); // 100, 200
```

>02.对于数组和对象，都是**Reference**

```javascript
// Let's say we have an array
const players = ['Wes', 'Sarah', 'Ryan', 'Poppy'];

// and we want to make a copy of it.
const team = players

console.log(players, team); //['Wes', 'Sarah', 'Ryan', 'Poppy'], ['Wes', 'Sarah', 'Ryan', 'Poppy']

// You might think we can just do something like this:
team[3] = 'Lux';

console.log(players, team); //['Wes', 'Sarah', 'Ryan', 'Lux'], ['Wes', 'Sarah', 'Ryan', 'Lux']
```

>03.以下列出几个复制数组的方法。---**Reference**

- `array.prototype.slice`:slice会返回新的数组。

   splice vs slice

  ```javascript
  slice(begin, end):

  ['a', 'b', 'c', 'd'].slice(1, 3) //['b'. 'c'] 是切片后的新数组。end不包含该项。

  splice(index, howmany, additem):

  x = ['a', 'b', 'c', 'd'];

  y = x.splice(1, 3)

  console.log(x) //['a']

  console.log(y) //['b', 'c', 'd']
  ```

- `concat`:可以创建一个空的数组，并添加其他数组。会返回新的数组。

- 利用`ES6 的`Array.from()`：将类数组转换成数组。

  ```javascript
  const bar = ["b", "a", "r"];
  Array.from(bar);
  // ["b", "a", "r"]

  Array.from('foo')
  //['f', 'o', 'o'];
  ```

  ​

```javascript
const team2 = players.slice();
const team3 = [].concat(players);
const team4 = [...players];
const team5 = Array.from(players);
```

>04.复制对象的方法。---**Reference**

- `Object.assign({}, object)`，但是assign() 只是浅拷贝，不能复制深层对象。
- `obj3 = JSON.parse(JSON.stringify(obj));`可以完整地复制对象。
  - `JSON.stringigy`：对象转换成字符串。
  - `JSON.parse`：字符串转换成对象。

```javascript
const obj1 = {
      name: 'Wes',
      age: 100,
      social: {
        twitter: '@wesbos',
        facebook: 'wesbos.developer'
      }
    };
obj2 = Object.assign({}, obj1, {age:12}); //one level copy
obj3 = JSON.parse(JSON.stringify(obj1)); //full copy
```
