# Key Sequence Detection

## 摘要

Key Sequence关键字序列：输入一段密码后会出现特定的画面。
本篇将侦测`Key Sequence`并产生特定画面。

## 重点

>01. 定义一个数组存储每次键入的内容，另一个存储密码。当两个数组匹配即可产生特定内容。

- 监听器用`window.addEventListener()`
- `keyup`事件:按下键盘时触发。
- `Array.prototyope.push`

```javascript
const pressed = [];
const secretCode = 'html';

window.addEventListener('keyup', (e) => {
  pressed.push(e.key);
  console.log(pressed);
})
```

console.log(pressed)控制台观察。

>02. 限制pressed长度与secretCode一样，并把旧的内容弹出。

- `Array.prototype.splice`:(start, deleteCount, additem)。
  - `start`:起始位置，负数表示倒数。
  - `deleteCount`: 删除长度。
  - `additem`:从起始位置开始，要增加的元素。
- 思路:
  - 当`start`为`-1`时，起始位置表示最后一个，此时不会有元素删除，当起始位置为-2时才会删除最后一个；同理若要刪除第一个元素，则需设置起始位置为`-n -1`。即第一个元素的前面一个位置。
  - 当`deleteCount`为负时不会删除。因此长度可以设置为`pressed.length-secret.length`，一开始keyup的东西不多时，为负数，不会删除。

```javascript
pressed.splice(-secretCode.length -1, pressed.length - secretCode.length);
```

>03. 判断pressed和secretCode内容是否匹配，并呈现特定画面。

- 用`.join()`方法把数组转换为字符串，
- 用`.includes()`方法判断是否存在内容。

```javascript
if (pressed.join('').includes(secretCode)){
  console.log('DING DIND');
  cornify_add();//特定画面。
}
```


