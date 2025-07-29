# Flex布局

使用`display: flex 或 display: inline-flex`

## 容器属性（作用于父元素）

### 1.flex-direction

设置主轴方向（决定子项排列方向）

```css
flex-direction: row;        /* 默认，主轴为水平方向，从左到右 */
flex-direction: row-reverse;/* 水平反向 */
flex-direction: column;     /* 主轴为垂直方向，从上到下 */
flex-direction: column-reverse;
```

### 2.flex-wrap

是否允许子项换行

```css
flex-wrap: nowrap;       /* 默认，不换行 */
flex-wrap: wrap;         /* 换行，从上到下 */
flex-wrap: wrap-reverse; /* 反向换行 */
```

### 3.flex-flow

`flex-direction` + `flex-wrap` 的简写形式

```css
flex-flow: row wrap;
```

### 4.`justify-content`（主轴对齐）

控制主轴上的子项排列方式

```css
justify-content: flex-start;    /* 默认，左对齐 */
justify-content: flex-end;      /* 右对齐 */
justify-content: center;        /* 居中 */
justify-content: space-between; /* 两端对齐，中间等距 */
justify-content: space-around;  /* 每个子项左右都有间距 */
justify-content: space-evenly;  /* 所有间距完全相等 */
```

### 5.`align-items`（交叉轴对齐）

设置子项在交叉轴（垂直）上的对齐方式（单行）

```css
align-items: stretch;    /* 默认，拉伸填满交叉轴 */
align-items: flex-start; /* 顶部对齐 */
align-items: flex-end;   /* 底部对齐 */
align-items: center;     /* 垂直居中 */
```

### 6.`align-content`（多行交叉轴对齐）

作用于多行内容的垂直对齐（仅当换行时生效）

```css
align-content: flex-start;
align-content: flex-end;
align-content: center;
align-content: space-between;
align-content: stretch;  /* 默认：填满容器 */
```

## 子项属性（作用于子元素）

### 1.`order`

设置项目显示顺序（数值越小越靠前）

```css
order: 0; /* 默认 */
```

### 2.`flex-grow`

子项在有剩余空间时如何放大（按比例）

```css
flex-grow: 1;  /* 平均分配剩余空间 */
flex-grow: 0;  /* 不放大 */
```

### 3.flex-shrink

```css
flex-shrink: 1; /* 默认 */
flex-shrink: 0; /* 不缩小 */
```

### 4.flex-basis

设置子项的初始宽度（或高度），相对于主轴而言，

```css
flex-basis: 200px;
```

### 5.`flex`（简写）

综合 `flex-grow shrink basis`

```css
flex: 1;             /* = 1 1 0% */
flex: 0 1 auto;      /* 默认值 */
flex: none;          /* = 0 0 auto */
```

### 6.align-self

设置单个子项在交叉轴上的对齐方式（覆盖 `align-items`）

```css
align-self: center;
```

