# DOM操作

`DOM`（Document Object Modal）文档对象模型

## 1.节点操作

### 1.1查询节点

```html
<div id="app" class="box"></div>
<span name="item"></span>

<script>
const el1 = document.getElementById("app")
const el2 = document.getElementsByClassName("box")
const el3 = document.querySelector(".box")
const el4 = document.querySelectorAll("span")
const el5 = document.getElementsByName("item")

console.log(el1, el2, el3, el4)
</script>
```

不常用

```html
element.parentElement 查询父节点
element.nextElementSibling 下一个节点
```

### 1.2创建节点

| **方法**                            | **描述**                                        | **返回值**                       | **示例用途**                                              |
| ----------------------------------- | ----------------------------------------------- | -------------------------------- | --------------------------------------------------------- |
| `document.createElement(tagName)`   | 创建一个指定的 HTML **元素**节点。              | 新创建的 Element 对象。          | 创建 `<div>`, `<p>`, `<img>` 等标签。                     |
| `document.createTextNode(data)`     | 创建一个包含指定文本内容的 **文本**节点。       | 新创建的 Text 对象。             | 插入到元素内部的纯文本内容。                              |
| `document.createComment(data)`      | 创建一个包含指定文本内容的 **注释**节点。       | 新创建的 Comment 对象。          | 插入 HTML 注释。                                          |
| `document.createDocumentFragment()` | 创建一个空的 **文档片段**（DocumentFragment）。 | 新创建的 DocumentFragment 对象。 | 批量创建和操作节点，然后一次性添加到 DOM 树，以提高性能。 |

```js
// 1. 创建一个新的 <p> 元素节点
const newParagraph = document.createElement('p');
// 2. 创建一个文本节点
const textContent = document.createTextNode('这是使用 DOM 操作创建的新段落！');
// 3. 将文本节点添加到 <p> 元素中
newParagraph.appendChild(textContent);

// (可选) 设置元素的属性，例如 ID 或 class
newParagraph.id = 'new-element';
newParagraph.style.color = 'blue';

```

如果你需要创建和插入**大量**节点（例如在一个循环中），**频繁地**直接操作 DOM 树会触发多次重绘和回流，严重影响性能。处理：

1. 创建一个 DocumentFragment。
2. 将所有新节点添加到这个片段中。
3. **最后**，将 DocumentFragment 一次性添加到 DOM 树中。

```js
// 创建文档片段
const fragment = document.createDocumentFragment();

for (let i = 0; i < 5; i++) {
    const listItem = document.createElement('li');
    listItem.textContent = `列表项 ${i + 1}`;
    // 将 <li> 添加到文档片段中，此时 DOM 树没有变化
    fragment.appendChild(listItem);
}

// 假设页面中有一个 <ul> 元素
const ulContainer = document.getElementById('myList');

// 一次性将文档片段中的所有节点移动/插入到 <ul> 元素中
// 只需要一次 DOM 操作，性能更高
ulContainer.appendChild(fragment);
```

### 1.3插入节点

**父节点操作子节点**

| **方法**                                              | **描述**                                                     | **用法**                                     |
| ----------------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------- |
| **`parentNode.appendChild(newNode)`**                 | 将 `newNode` 添加到 `parentNode` 的**子节点列表的末尾**。    | `container.appendChild(newDiv);`             |
| **`parentNode.insertBefore(newNode, referenceNode)`** | 将 `newNode` 插入到 `parentNode` 内的 `referenceNode` **之前**。 | `container.insertBefore(newDiv, existingP);` |
| **`parentNode.replaceChild(newNode, oldNode)`**       | 用 `newNode` 替换掉 `parentNode` 内的 `oldNode`。            | `container.replaceChild(newDiv, oldP);`      |

```js
//父节点container 子节点newNode 目标节点targetNode
container.appendChild(newNode);   //子节点插入父节点内部最后
container.insertBefore(newNode, targetNode); //子节点插入父节点内部目标节点前面
container.replaceChild(newNode, targetP); //子节点替换父节点内部目标节点

container.append(...nodesOrStrings) //多个节点插入父节点内部末尾
container.prepend(...nodesOrStrings) //多个节点插入父节点内部子列表节点开头
```

**基于自身节点（兄弟节点）**

```js
targetNode.before(...nodesOrStrings)  //多个节点插入targetNode之前
targetNode.after(...nodesOrStrings)   //多个节点插入targetNode之后
```

### 1.4删除节点

```js
const el = document.getElementById("app")
el.remove()  // 直接删除
```

## 2.文本操作

```js
const el = document.querySelector(".box")

el.innerHTML = "<b>加粗内容</b>"
el.innerText = "<b>不会解析成 HTML</b>"
el.textContent = "普通文本"
```

`innerHTML`包含解析的HTMl标签

`innerText`忽略所有 HTML 标签，只保留文本。

## 3.操作class

```js
const el = document.querySelector(".box")

el.classList.add("active", "highlight")  //为元素添加 active 和 highlight 类。
el.classList.remove("active")            //移除 active 类。
el.classList.toggle("show")              //如果元素有 show，则移除；否则，添加。
el.classList.replace('old', 'new');      //将 old 类名替换为 new 类名。
console.log(el.classList.contains("show"))  //是否包含show类名
```

## 4.元素尺寸位置

```js
const box = document.querySelector(".box")

console.log(box.offsetWidth)    // 含边框
console.log(box.clientWidth)    // 不含边框
console.log(box.getBoundingClientRect())  // 精确位置
```

## 5.事件操作

```js
const button = document.getElementById('myButton');

// 示例 1: 绑定点击事件
button.addEventListener('click', function(event) {
    console.log('按钮被点击了!');
});

// 示例 2: 绑定鼠标悬停事件
button.addEventListener('mouseover', () => {
    button.style.backgroundColor = 'lightblue';
});

//监听dom是否渲染完成
document.addEventListener("DOMContentLoaded", () => {
  const cells = document.querySelectorAll("[data-cell]");
  console.log(cells);
});

```

移除事件

```js
const button = document.getElementById('myButton');
// 1. 定义一个具名函数
function handleClick(event) {
    console.log('执行一次后移除');
}
// 2. 绑定事件
button.addEventListener('click', handleClick);
// 3. 移除事件
button.removeEventListener('click', handleClick);
```







