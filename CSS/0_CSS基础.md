# CSS基础

## 定位

正常文档流：`static`，`relative`（相对定位），`sticky`（粘性定位）

脱离文档流：`fixed`（固定定位），`absolute`（绝对定位）

控制方位：`left`，`right`，`top`，`bottom`

```css
position: relative;
left: 10px;
```

- `relative`：相对于自身作参照物，不脱离文档流
- `absolute`：参照物为最近的一个非static的父容器；如果当前父级未设置定位，会向上层找到；如果都没有找到则以浏览器左上角为参照物。脱离文档流会影响其他元素的布局。
- `fixed`：参照物始终为浏览器左上角，脱离文档流会影响其他元素的布局
- `sticky`：正常情况，正常布局。当滚动区域超过预设的方位值，表现fixed定位





