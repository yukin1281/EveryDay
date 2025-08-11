# Vue2常用修饰符

## 事件修饰符

```html
<button @click.prevent.stop="handleClick">点击</button>
```

| 修饰符     | 作用说明                                                     |
| ---------- | ------------------------------------------------------------ |
| `.stop`    | 调用 `event.stopPropagation()`，阻止事件冒泡                 |
| `.prevent` | 调用 `event.preventDefault()`，阻止默认行为                  |
| `.capture` | 添加事件监听器时使用捕获模式                                 |
| `.self`    | 只有在事件是由自身元素触发时才执行回调                       |
| `.once`    | 事件只触发一次                                               |
| `.passive` | 告诉浏览器事件监听器不会调用 `preventDefault()`（提高性能，常用于滚动） |

链式组合使用

```html
<button @click.stop.prevent="handleClick">提交</button>
表示先阻止冒泡，再阻止默认行为。
```

## 表单修饰符

```html
<input v-model.lazy.trim.number="msg" />
```

| 修饰符    | 作用说明                                                |
| --------- | ------------------------------------------------------- |
| `.lazy`   | 将 `input` 事件改为 `change` 事件触发更新（失焦后更新） |
| `.number` | 自动将输入值转换为数字类型                              |
| `.trim`   | 自动去除首尾空格                                        |

## 自定义事件更新修饰符（配合 `.sync`）

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

| 修饰符   | 作用说明                                          |
| -------- | ------------------------------------------------- |
| `.prop`  | 将绑定内容作为 DOM 属性（而不是 HTML 特性）绑定   |
| `.camel` | 将 kebab-case 转为 camelCase（主要用于 SVG 属性） |
| `.sync`  | 启用双向绑定的语法糖（见前文详解）                |

## 按键修饰符（用于 `v-on` 键盘事件）

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