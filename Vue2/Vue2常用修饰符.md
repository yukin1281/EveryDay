# Vue2常用修饰符

## 事件修饰符

```html
<button @click.prevent.stop="handleClick">点击</button>
```

| 修饰符     | 作用说明                                                     |
| ---------- | ------------------------------------------------------------ |
| `.stop`    | 调用 `event.stopPropagation()`，阻止事件冒泡                 |
| `.prevent` | 调用 `event.preventDefault()`，阻止默认行为                  |
| `.capture` | 添加事件监听器时使用捕获模式，使事件触发从包含这个元素的顶层开始往下触发 |
| `.self`    | 只有在事件是由自身元素触发时才执行回调                       |
| `.once`    | 事件只触发一次                                               |
| `.passive` | 告诉浏览器事件监听器不会调用 `preventDefault()`（提高性能，常用于滚动） |
| `.native`  | 让组件变成像`html`内置标签那样监听根元素的原生事件，否则组件上使用 `v-on` 只会监听自定义事件 |

链式组合使用

```html
<button @click.stop.prevent="handleClick">提交</button>
表示先阻止冒泡，再阻止默认行为。
```

`.capture`

```javascript
<div @click.capture="shout(1)">
    obj1
    <div @click.capture="shout(2)">
        obj2
        <div @click="shout(3)">
            obj3
            <div @click="shout(4)">
                obj4
            </div>
        </div>
    </div>
</div>
// 点击4 输出结构: 1 2 4 3 
// 事件流（捕获->目标->冒泡）
// 捕获阶段 从外到内，执行capture修饰监听，输出1,2
// 目标阶段 输出4，触发冒泡监听
// 冒泡阶段 输出3
```

`.passive`

在移动端，当我们在监听元素滚动事件的时候，会一直触发`onscroll`事件会让我们的网页变卡，因此我们使用这个修饰符的时候，相当于给`onscroll`事件整了一个`.lazy`修饰符

```javascript
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

`.native`

```vue
<my-component v-on:click.native="doSomething"></my-component>
<el-button @click.native="doSomething"></el-button>
```



## 表单修饰符

```html
<input type="text" v-model.lazy="value">
<p>{{value}}</p>
<input type="text" v-model.trim="value">
<input v-model.number="age" type="number">
```

| 修饰符    | 作用说明                                                     |
| --------- | ------------------------------------------------------------ |
| `.lazy`   | 将 `input` 事件改为 `change` 事件触发更新（失焦后更新）      |
| `.number` | 自动将输入值转换为数字类型，但如果这个值无法被`parseFloat`解析，则会返回原来的值 |
| `.trim`   | 自动去除首尾空格，而中间的空格不会过滤                       |

## 自定义事件更新修饰符（配合 `.sync`）

`.sync`

```html
<child :title.sync="pageTitle" />
等同于
<child :title="pageTitle" @update:title="val => pageTitle = val" />
```

子组件中

```html
this.$emit('update:title', '新标题')
```

将 prop 的传值和子组件的事件自动绑定在一起，**实现父值更新的“语法简写”**。

### v-bind 修饰符

| 修饰符   | 作用说明                                                     |
| -------- | ------------------------------------------------------------ |
| `.prop`  | 将绑定内容作为 DOM 属性（而不是 HTML 特性）绑定              |
| `.camel` | 将 kebab-case 转为 camelCase（主要用于 SVG 属性）将命名变为驼峰命名法 |
| `.sync`  | 启用双向绑定的语法糖（见前文详解）                           |

`.prop`

设置自定义标签属性，避免暴露数据，防止污染HTML结构

```js
<input id="uid" title="title1" value="1" :index.prop="index">
```

`.camel`

```javascript
<svg :viewBox="viewBox"></svg>
```

## 鼠标按钮修饰符

鼠标按钮修饰符针对的就是左键、右键、中键点击，有如下：

- left 左键点击
- right 右键点击
- middle 中键点击

```js
<button @click.left="shout(1)">ok</button>
<button @click.right="shout(1)">ok</button>
<button @click.middle="shout(1)">ok</button>
```

## 键盘按键修饰符（用于 `v-on` 键盘事件）

```html
<input @keyup.enter="submit" />
```

| 修饰符    | 说明                        |
| --------- | --------------------------- |
| `.enter`  | 回车键                      |
| `.tab`    | Tab 键                      |
| `.delete` | 删除键（delete、backspace） |
| `.esc`    | ESC 键                      |
| `.space`  | 空格键                      |
| `.up`     | 上方向键                    |
| `.down`   | 下方向键                    |
| `.left`   | 左方向键                    |
| `.right`  | 右方向键                    |