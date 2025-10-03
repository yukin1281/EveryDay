# JS类型转换

javascript数据类型分为基本类型和引用类型，**基本类型（原始类型）**包括`string`，`number`，`boolean`，`null`，`undefined`，`symbol`，`bigint`

**引用类型**包括`Object`（`Array`，`Function`，`Date`，`RegExp`等）

## `==`和`===`的区别

```js
console.log(1 == '1'); // true
console.log(1 === '1'); // false

```

- `==`会发生隐式类型转换，只判断值是否相等。
- `===`不会发生隐式类型转换，值和类型都要相等才为true。

### `==`转换规则

- `null` == `undefined` // true
- `null` 和 `undefined` 与其他值比较时不转换为数字
- `NaN` 与任何值比较都是 `false`（包括 `NaN` 本身）
- 对象与原始值比较 → `ToPrimitive` 后再比较

```js
[] == 0          // true  ([] → "" → 0)
[] == false      // true  ([] → "" → 0, false → 0)
[1] == 1         // true  ([1] → "1" → 1)
NaN == NaN       // false
NaN !== NaN      // true
isNaN('foo')     // true  (因为 'foo' → NaN)
Number.isNaN('foo') // false (不会隐式转换)
{} == 0   //false   // ({} → "[object Object]" → NaN) NaN == 0 false
```

## 类型转换两大方式

### 显式类型转换

**显式调用**一些方法/函数手动将一个值转换为另一种类型

- 转字符串：`String(value)`，`value.toString() (null/undefined调用会报错)`，`JSON.stringify(value)`
- 转数字：`Number(value)`，`parseInt(value)/parseFloat(value)（只对字符串有用，遇到非数字字符停止解析）`
- 转布尔值：`Boolean(value)`，`!!value`

```js
console.log(String(456)); // '456'
console.log(Number('123')); // 123
console.log(Number(undefined)); // NaN
console.log(Boolean(0)); // false
console.log(parseInt('')) //NaN 只能解析数字字符串
console.log(parseInt(null)) //NaN
```

### 隐式类型转换

由运算符或上下文自动触发，不需要你手动调用。

- 字符串拼接：`'a' + 1  // 'a1'`
- 算术运算：`'5' - 2  // 3`  `（* / % 同理将字符串转换为数字）`
- 比较运算：`==` 会进行类型转换，`===` 不会
- 条件判断：`if (value)` 会将 value 转为布尔值
- 对象参与运算时调用 `ToPrimitive`（`valueOf` → `toString`）

## 基本类型和引用类型转换

**1、基本类型转基本类型**

- 直接用`Boolean(x)`、`Number(x)`、`String(x)`即可。

**2、引用类型转基本类型**

- 转布尔：任何对象转布尔都为`true`
- 转字符串：`String(obj)`会调用`ToPrimitive(obj, String)`
- 转数字：`Number(obj)`会调用`ToPrimitive(obj, Number)`

## ToPrimitive原理

`ToPrimitive(obj,hint)`这里`hint`有三种：

- `string` → 优先调用 `toString()`，失败再调用 `valueOf()`
- `number` → 优先调用 `valueOf()`，失败再调用 `toString()`

- `default` → 大部分对象的行为和 `number` 相同（优先 `valueOf()`），但 `Date` 是个特例（默认走 `string`）

以上转换完成之后仍不是基本类型则抛出异常

```js
let obj = {
  valueOf() { return 42; },
  toString() { return "hello"; }
};

console.log(String(obj)); // "hello" (hint="string")
console.log(Number(obj)); // 42      (hint="number")

console.log(obj + 1);     // 43      (hint="default"≈number, valueOf优先)
console.log(obj + '');    // "42"    (hint="default"≈number, valueOf优先)

console.log(new Date() + ''); // 类似 "Tue Aug 19 2025 ..." (hint="default"≈string, toString优先)

！！！注意点：
对象调用valueOf()默认返回对象本身this，并不会自动将对象转换为原始类型，取决于这个对象有没有重写valueOf方法
let obj = {};
console.log(obj.valueOf() === obj); // true
```

**口诀**

- **String 转换 → toString 优先**
- **Number 转换 → valueOf 优先**
- **加号运算 → 默认走 valueOf 优先**（除了 `Date`）

## valueOf与toString

### `valueOf()`

| 对象       | 返回值                                                    |
| ---------- | --------------------------------------------------------- |
| `Array`    | 返回数组对象本身。                                        |
| `Boolean`  | 布尔值。                                                  |
| `Date`     | 存储的时间是从`1970`年`1`月`1`日午夜开始计的毫秒数`UTC`。 |
| `Function` | 函数本身。                                                |
| `Number`   | 数字值。                                                  |
| `Object`   | 默认情况下返回对象本身。                                  |
| `String`   | 字符串值。                                                |

```js
var arr = [];
console.log(arr.valueOf() === arr); // true

var date = new Date();
console.log(date.valueOf()); // 1376838719230

var num =  1;
console.log(num.valueOf()); // 1

var bool = true;
console.log(bool.valueOf() === bool); // true

var newBool = new Boolean(true);
console.log(newBool.valueOf() === newBool); // false // 前者是bool 后者是object
console.log(typeof newBool.valueOf(), typeof newBool) //boolean object

function funct(){}
console.log(funct.valueOf() === funct); // true

var obj = {};
console.log(obj.valueOf() === obj); // true

var str = "";
console.log(str.valueOf() === str); // true

var newStr = new String("");
console.log(newStr.valueOf() === newStr); // false // 前者是string 后者是object
console.log(typeof newStr.valueOf()， typeof newStr) //string object
```

### `toString()`

| 对象       | 返回值                                                       |
| ---------- | ------------------------------------------------------------ |
| `Array`    | 以逗号分割的字符串，如`[1,2]`的`toString`返回值为`1,2`。     |
| `Boolean`  | 布尔值的字符串形式。                                         |
| `Date`     | 可读的时间字符串，例如`Tue Oct 27 2020 16:08:48 GMT+0800` (中国标准时间) |
| `Function` | 声明函数的`Js`源代码字符串。                                 |
| `Number`   | 数字值的字符串形式。                                         |
| `Object`   | `[object Object]`字符串。                                    |
| `String`   | 字符串。                                                     |

```js
ar arr = [1, 2, 3];
console.log(arr.toString()); // 1,2,3

var date = new Date();
console.log(date.toString()); // Tue Oct 27 2020 16:12:35 GMT+0800 (中国标准时间)

var num =  1;
console.log(num.toString()); // 1

var bool = true;
console.log(bool.toString()); // true

function funct(){ var a = 1; }
console.log(funct.toString()); // function funct(){ var a = 1; }

var obj = {a: 1};
console.log(obj.toString()); // [object Object]

var str = "1";
console.log(str.toString()); // 1
```

## ToString

- `null` → `'null'`
- `undefined` →`'undefined'`
- 布尔值`true`→`'true'`，`false`→`'false'`
- 数字转字符串
- 对象→调用`toString()`，默认`[object Object]`

## ToNumber

- `null`→`0`
- `undefined`→`NaN`
- 布尔值`true`→1，`false`→0
- 字符串→按规则解析（空字符串为0，非数字为`NaN`）
- 对象→先`ToPrimitive`再`ToNumber`

## ToBoolean

**假值（falsy）**有8种：`false`，`0`，`-0`，`0n(BigInt零)`，`''(空字符串)`，`null`，`undefined`，`NaN`

其余都为真值（truthy）

```js
Boolean({}) // true
Boolean([]) // true
```

## 参考

```shell
https://juejin.cn/post/7419907933256794131
https://juejin.cn/post/7520073195183210548#heading-11
https://cloud.tencent.com/developer/article/2396339
```



