# CSS选择器

## 基本选择器

| 选择器   | 描述                 | 示例                          |
| -------- | -------------------- | ----------------------------- |
| `*`      | 通配符，选中所有元素 | `* { margin: 0; }`            |
| `tag`    | 选中所有指定标签     | `p { color: red; }`           |
| `.class` | 选中指定类名的元素   | `.box { border: 1px solid; }` |
| `#id`    | 选中指定 ID 的元素   | `#main { width: 100%; }`      |

## 组合选择器

| 选择器  | 描述                  | 示例                            |
| ------- | --------------------- | ------------------------------- |
| `A B`   | 选中 A 内所有 B       | `div p { font-size: 14px; }`    |
| `A > B` | 选中 A 的直接子元素 B | `ul > li { list-style: none; }` |
| `A + B` | 选中紧跟在 A 后的 B   | `h1 + p { margin-top: 0; }`     |
| `A ~ B` | 选中 A 之后所有同级 B | `h1 ~ p { color: gray; }`       |

## 属性选择器

| 选择器         | 描述              | 示例                                   |
| -------------- | ----------------- | -------------------------------------- |
| `[attr]`       | 含有某属性的元素  | `[disabled] { opacity: 0.5; }`         |
| `[attr=value]` | 属性值等于指定值  | `[type="text"] { width: 200px; }`      |
| `[attr^=val]`  | 属性值以 val 开头 | `[href^="https"] { color: green; }`    |
| `[attr$=val]`  | 属性值以 val 结尾 | `[src$=".png"] { border: 1px solid; }` |
| `[attr*=val]`  | 属性值包含 val    | `[title*="提示"] { cursor: help; }`    |

## 伪类选择器

| 选择器           | 描述             | 示例                                    |
| ---------------- | ---------------- | --------------------------------------- |
| `:hover`         | 鼠标悬停         | `a:hover { color: red; }`               |
| `:focus`         | 获得焦点         | `input:focus { outline: none; }`        |
| `:first-child`   | 第一个子元素     | `li:first-child { font-weight: bold; }` |
| `:last-child`    | 最后一个子元素   | `li:last-child { color: gray; }`        |
| `:nth-child(n)`  | 第 n 个子元素    | `li:nth-child(2) { color: blue; }`      |
| `:not(selector)` | 不匹配指定选择器 | `div:not(.active) { display: none; }`   |

## 伪元素选择器

| 选择器           | 描述           | 示例                                   |
| ---------------- | -------------- | -------------------------------------- |
| `::before`       | 元素前插入内容 | `p::before { content: "▶ "; }`         |
| `::after`        | 元素后插入内容 | `p::after { content: " ✓"; }`          |
| `::first-letter` | 第一个字母     | `p::first-letter { font-size: 200%; }` |
| `::first-line`   | 第一行         | `p::first-line { color: red; }`        |