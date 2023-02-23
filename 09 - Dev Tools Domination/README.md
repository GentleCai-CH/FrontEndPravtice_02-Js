# Dev tools 

## 摘要

本篇主要介绍console的几个常用方法，可以增加开发效率。

## 重点

> 01.输出打印`console.log()`
  - 应用字符串格式化：`console.log('hello %s', 'world');`
  - 应用css样式：`console.log('%c hello world', 'font-size:50px');`
- 其他显示:
  - `console.warn('hello world')`:警告显示
  - `console.error('hello world')`:错误显示
  - `console.info('hello world')`:信息显示

> 02.断言测试

- `console.assert()`若第一参数为`false`，則会输出第二参数。`console.assert( 1 ===2 , 'This is false');`。

> 03.清除画面

- `console.clear()`:清除console画面。

> 04.查看dom元素的属性

- `console.dir()`:可以查看被选取的dom元素的属性。

> 05.Group信息。

```javascript
   dogs.forEach(dog => {
      console.groupCollapsed(`${dog.name}`);
      console.log(`This is ${dog.name}`);
      console.log(`${dog.name} is ${dog.age} years old`);
      console.log(`${dog.name} is ${dog.age * 7} dog years old`);
      console.groupEnd(`${dog.name}`);
    });
```

> 06. 计算次数

- `console.count()`:可以计算参数出现的次数。

> 07.计算执行时间

- 可以计算`console.time('hello world')`到`console.timeEnd('hello world')`的执行时间。

> 08.将array转换成table形式

- `console.table(dog)`:把array用表格形式输出。
