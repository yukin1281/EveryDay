# Vue3学习笔记

## 环境搭建

### 基于vite创建

```shell
npm create vue@latest
```

### 基于vue-cli创建

```shell
vue create vue_test
```

## 组件

组件是可复用的 Vue 实例, 如果网页中的某一个部分需要在多个场景中使用，那么我们可以将其抽出为一个组件进行复用。组件大大提高了代码的复用率。

### 引入element-plus组件

1.**安装 Element Plus**：在项目目录中，使用 npm 安装 Element Plus：

```bash
npm install element-plus
```

2.**引入 Element Plus**：在 `main.js` 中引入 Element Plus：

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

const app = createApp(App)
app.use(ElementPlus)
app.mount('#app')
```



