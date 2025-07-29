# Grid布局

使用：`display: grid;` 或 `display: inline-grid;`

## 容器属性（定义网格规则）

### 1. `grid-template-columns` / `grid-template-rows`

定义列和行的轨道（宽度或高度）

```css
grid-template-columns: 100px 1fr 2fr; /* 三列 */
grid-template-rows: 100px auto;
```

### 2. `grid-template-areas`

使用命名区域布局

```css
grid-template-areas:
  "header header"
  "sidebar main";
```

并在子项中声明：

```css
grid-area: header;
```

### 3. `grid-auto-rows` / `grid-auto-columns`

自动生成的轨道的默认尺寸（用于隐式网格）

```css
grid-auto-rows: 100px;
```

### 4.`grid-auto-flow`

控制项目的自动放置方向（默认是按行）

```css
grid-auto-flow: row | column | row dense | column dense;
```

`dense`：启用**紧凑排列算法**，允许**后插入的小元素填补前面留下的空位**，从而减少空隙。

### 5.`gap`（或 `row-gap` 和 `column-gap`）

网格轨道间的间距

```css
gap: 20px;
```

## 子项属性（控制子项如何放置）

### 1.`grid-column` / `grid-row`

控制子项跨几列或几行（速写）

```css
grid-column: 1 / 3; /* 从第1列到第3列（不包含3） */
grid-row: 2 / span 2; /* 跨2行 */
```

或者 详细写法

```css
grid-column-start: 1;
grid-column-end: 3;
```

### 2.`grid-area`

```css
rid-area: 2 / 1 / 4 / 3; /* row-start / col-start / row-end / col-end */
```

### 3. `justify-self` / `align-self`

控制**单元格内**的**内容**对齐方式：

```css
justify-self: start | end | center | stretch;  //水平
align-self: start | end | center | stretch; //垂直
```

stretch表示子元素会被拉伸以填满主轴的高度或宽度。

### 4.`place-self`（简写）

`justify-self`和`align-self`二合一

```css
place-self: center stretch;
```

## 网格对齐（对所有子项）

- `justify-items`: 控制每个子项在**水平方向**的对齐方式
- `align-items`: 控制每个子项在**垂直方向**的对齐方式
- `place-items`: 简写
- `justify-content`: 控制整个网格在**主轴**上的对齐方式
- `align-content`: 控制整个网格在**交叉轴**上的对齐方式
- `place-content`: 简写

## 常用单位与函数

| 单位/函数                | 说明                                     |
| ------------------------ | ---------------------------------------- |
| `fr`                     | 自适应剩余空间，类似 Flex 的 `flex-grow` |
| `repeat()`               | 重复某列/行规则，例如 `repeat(3, 1fr)`   |
| `minmax()`               | 设置一个范围，如 `minmax(200px, 1fr)`    |
| `auto-fill` / `auto-fit` | 响应式自动铺满容器                       |

```css
.container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}
```

