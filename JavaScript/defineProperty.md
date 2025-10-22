# defineProperty

`Object.defineProperty()`方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象，也就是说，该方法允许精确地添加或修改对象的属性。

## 语法

```js
Object.defineProperty(obj, prop, descriptor)
```

- `obj`：目标对象
- `prop`：要定义的属性名（字符串或`Symbol`）
- `descriptor`：要定义或修改的属性描述符对象

## 属性描述符

描述符分为两类：数据描述符和访问器描述符。一个描述符只能是数据描述符和存取描述符这两者其中之一，（不能同时含 `value`/`writable` 与 `get`/`set`，否则会抛出 `TypeError`），否则抛出异常。

数据描述符（包含 `value` / `writable`），内部槽：`[[Value]]`、`[[Writable]]`、`[[Enumerable]]`、`[[Configurable]]`。

访问器描述符（包含 `get` / `set`），内部槽：`[[Get]]`、`[[Set]]`、`[[Enumerable]]`、`[[Configurable]]`。

- `value`：属性的值（data descriptor）。
- `writable`：`true` 可赋值修改（data descriptor）。默认 `false`（见默认行为）。
- `get`：无参数函数，读取属性时调用（accessor descriptor）。
- `set`：有一个参数函数，写入属性时调用（accessor descriptor）。
- `enumerable`：是否在 `for...in` / `Object.keys()` / `JSON.stringify` 中可枚举。默认 `false`。
- `configurable`：是否允许修改属性描述或删除属性。默认 `false`。

`Object.defineProperty()`默认行为，如果只写 `value` 而未显式声明 `writable`/`enumerable`/`configurable`，这些都会 **默认为 `false`**。
 这与直接通过赋值创建属性（`obj.x = 1`，其属性默认是可写、可枚举、可配置）不同。

```js
const a = {};
a.x = 1;
console.log(Object.getOwnPropertyDescriptor(a, 'x'));
// { value: 1, writable: true, enumerable: true, configurable: true }

const b = {};
Object.defineProperty(b, 'y', { value: 2 });
console.log(Object.getOwnPropertyDescriptor(b, 'y'));
// { value: 2, writable: false, enumerable: false, configurable: false }
```

## 属性描述符用法

### configurable

当且仅当该属性的`configurable`键值为`true`时，该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除，默认为`false`，默认值是指在使用`Object.defineProperty()`定义属性时的默认值。

```js
"use strict";
// 非严格模式下操作静默失败，即不报错也没有任何效果
// 严格模式下操作失败抛出异常

var obj = {};
Object.defineProperty(obj, "key", {
    configurable: true,
    value: 1
});
console.log(obj.key); // 1
Object.defineProperty(obj, "key", {
    enumerable: true // configurable为true时可以改变描述符
});
delete obj.key; // configurable为true时可以删除属性
console.log(obj.key); // undefined
```

```js
"use strict";
var obj = {};
Object.defineProperty(obj, "key", {
    configurable: false,
    value: 1
});
console.log(obj.key); // 1
Object.defineProperty(obj, "key", {
    enumerable: true // configurable为false时不可以改变描述符 // Uncaught TypeError: Cannot redefine property: key
});
delete obj.key; // configurable为false时不可以删除属性 // Uncaught TypeError: Cannot delete property 'key' of #<Object>
// 代码停止执行
console.log(obj.key); // undefined
```

### enumerable

当且仅当该属性的`enumerable`键值为`true`时，该属性才会出现在对象的枚举属性中，默认为 `false`。

```javascript
"use strict";
var obj = { a: 1 };
Object.defineProperty(obj, "key", {
    enumerable: true,
    value: 1
});
for(let item in obj) console.log(item, obj[item]); 
/* 
  a 1
  key 1
*/
```

```js
"use strict";
var obj = { a: 1 };
Object.defineProperty(obj, "key", {
    enumerable: false,
    value: 1
});
for(let item in obj) console.log(item, obj[item]); 
/* 
  a 1
*/
```

### value

该属性对应的值，可以是任何有效的`JavaScript`值，默认为`undefined`。

```js
"use strict";
var obj = {};
Object.defineProperty(obj, "key", {
    value: 1
});
console.log(obj.key); // 1
```

```js
"use strict";
var obj = {};
Object.defineProperty(obj, "key", {});
console.log(obj.key); // undefined
```

### writable

当且仅当该属性的`writable`键值为`true`时，属性的值，才能被赋值运算符改变，默认为 `false`。

```js
"use strict";
var obj = {};
Object.defineProperty(obj, "key", {
    value: 1,
    writable: true
});
console.log(obj.key); // 1
obj.key = 11;
console.log(obj.key); // 11
```

```js
"use strict";
var obj = {};
Object.defineProperty(obj, "key", {
    value: 1,
    writable: false,
    configurable: true
});
console.log(obj.key); // 1
obj.key = 11; // Uncaught TypeError: Cannot assign to read only property 'key' of object '#<Object>'
Object.defineProperty(obj, "key", {
    value: 11 // 可以通过redefine来改变值，注意configurable需要为true
});
console.log(obj.key); // 11
```

### get

属性的`getter`函数，如果没有`getter`，则为`undefined`。当访问该属性时，会调用此函数，执行时不传入任何参数，但是会传入`this`对象，由于继承关系，这里的`this`并不一定是定义该属性的对象。该函数的返回值会被用作属性的值，默认为`undefined`。

```js
"use strict";
var obj = { __x: 1 };
Object.defineProperty(obj, "x", {
    get: function(){ return this.__x; }
});
console.log(obj.x); // 1
```

```js
"use strict";
var obj = { __x: 1 };
Object.defineProperty(obj, "x", {
    get: function(){ return this.__x; }
});
console.log(obj.x); // 1
obj.x = 11; // 没有set方法 不能直接赋值 // Uncaught TypeError: Cannot set property x of #<Object> which has only a getter
```

### set

属性的`setter`函数，如果没有`setter`，则为`undefined`。当属性值被修改时，会调用此函数，该方法接收一个参数，且传入赋值时的`this`对象，从而进行赋值操作，默认为`undefined`。

```js
"use strict";
var obj = { __x: 1 };
Object.defineProperty(obj, "x", {
    set: function(x){ this.__x = x; },
    get: function(){ return this.__x; }
});
obj.x = 11;
console.log(obj.x); // 11
```

```js
"use strict";
var obj = { __x: 1 };
Object.defineProperty(obj, "x", {
    set: function(x){ 
        console.log("在赋值前可以进行其他操作");
        this.__x = x; 
    }
});
obj.x = 11;
```

