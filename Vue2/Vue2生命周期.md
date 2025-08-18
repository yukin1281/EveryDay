# Vue2生命周期

`Vue`实例需要经过创建、初始化数据、编译模板、挂载`DOM`、渲染、更新、渲染、卸载等一系列过程，这个过程就是`Vue`的生命周期。

可分为4个阶段：

- 第一阶段（创建阶段）：`beforeCreate`，`created`
- 第二阶段（挂载阶段）：`beforeMount`，`mounted`
- 第三阶段（更新阶段）：`beforeUpdate`，`updated`
- 第四阶段（销毁阶段）：`beforeDestroy`，`destroyed`

## beforeCreate

官网：在实例初始化之后,进行数据侦听和事件/侦听器的配置之前同步调用。

这个阶段Vue实例对象（`this`）刚在内存中构造出来，内部还没有初始化操作。**DOM未渲染，也没初始化 `data`、`methods`、`computed` 等，不能访问`this.data`、`this.methods`**，适合开启全局loading、初始化一些非响应式数据。

```javascript
 beforeCreate() {
    console.log('beforeCreate: 实例创建前');
    console.log(this); //VueComponent {...}  当前Vue实例
    console.log(this.$el);  //undefined  当前组件实例对应的DOM元素根结点
    console.log(this.$data); //undefined 
    console.log(this.msg); // undefined
    console.log("--------------------");
},
```

## created

官网：在实例创建完成后被立即同步调用。在这一步中，实例已完成对选项的处理，意味着以下内容已被配置完毕：数据侦听、计算属性、方法、事件/侦听器的回调函数。然而，挂载阶段还没开始，且 `$el` property 目前尚不可用。

完成数据绑定的配置，计算属性与方法挂载，`watch/event`事件回调等（**即`data`、`methods`、`computed` 等都可用了），但还没生成 DOM，页面还未渲染**（`this.$el` 还是虚的）。适合发请求获取数据、初始赋值。

```javascript
 created() {
    console.log('created: 实例创建完成');
    console.log(this.$el);  //undefined  
    console.log(this.$data); //{message: "Hello, Vue!"}
    console.log(this.message); // "Hello, Vue!"
    console.log("--------------------");
},
```

出现响应式数据

```javascript
export default {
  data() {
    return { count: 0 }
  },
  beforeCreate() {
    console.log(this.count) // ❌ undefined
  },
  created() {
    console.log(this.count) // ✅ 0（已经是响应式数据）
    this.count++ // ✅ 会触发视图更新
  }
}
```

## beforeMount

官网：在挂载开始之前被调用：相关的 `render` 函数首次被调用。该钩子在服务器端渲染期间不被调用。

完成了页面模板的解析，在内存中将页面的数据与指令等进行解析，当页面解析完成，页面模板就存在于内存中。在此生命周期钩子执行时`$el`被创建，但是页面只是在内存中，并未作为`DOM`渲染。适合最后一次修改数据，马上就要渲染了。

```javascript
beforeMount() {
    console.log('beforeMount: 实例挂载前');
    console.log(this.$el);  // <div id="app">...</div>
    console.log(this.$data); //{message: "Hello, Vue!"}
    console.log(this.message); // "Hello, Vue!"
    console.log("--------------------");
},
```

## mounted

官网：实例被挂载后调用，这时 `el` 被新创建的 `vm.$el` 替换了。如果根实例挂载到了一个文档内的元素上，当 `mounted` 被调用时 `vm.$el` 也在文档内。注意 `mounted` **不会**保证所有的子组件也都被挂载完成。如果你希望等到整个视图都渲染完毕再执行某些操作，可以在 `mounted` 内部使用 [vm.$nextTick](https://v2.cn.vuejs.org/v2/api/#vm-nextTick)

```javascript
mounted: function () {
  this.$nextTick(function () {
    // 仅在整个视图都被渲染之后才会运行的代码
  })
}
```

完成页面从内存中渲染到`DOM`的操作，在此声明周期钩子执行时页面已经渲染完成，组件正式完成创建阶段的最后一个钩子，即将进入运行中阶段。渲染页面模板优先级：`render`函数 `>` `template`属性 `>` 外部`HTML`。

```javascript
mouted() {
    console.log('mounted: 实例挂载完成');
    console.log(this.$el);  // <div id="app">...</div>
    console.log(this.$data); //{message: "Hello, Vue!"}
    console.log(this.message); // "Hello, Vue!"
    console.log("--------------------");
},
```

## beforeUpdate

官网：**在数据发生改变后，DOM 被更新之前被调用**。这里适合在现有 DOM 将要被更新之前访问它，比如移除手动添加的事件监听器。

数据已更新，但DOM未更新。

```javascript
beforeUpdate() {
    console.log('beforeUpdate: 数据更新前');
    console.log(this.$el);  // <div id="app">...</div>
    console.log(this.$data); //{message: "Hello, Vue! Updated!"}
    console.log(this.message); // Hello, Vue! Updated!
    debugger
    console.log("--------------------");
},
```

可以看到`Vue`实例中数据已经是最新的，但是在页面中的数据还是旧的。

## updated

当数据发生更新并在`DOM`渲染完成后`updated`钩子便会被调用，在此时组件的`DOM`已经更新，可以执行依赖于`DOM`的操作。（即数据更新，DOM已重新渲染）

```javascript
updated() {
    console.log('updated: 数据更新后');
    console.log(this.$el);  // <div id="app">...</div>
    console.log(this.$data); //{message: "Hello, Vue! Updated!"}
    console.log(this.message); // "Hello, Vue! Updated!"    
},
```

## beforeDestroy

官网：实例销毁之前调用。在这一步，实例仍然完全可用。

实例即将被销毁，数据、事件、子组件都还在，适合做清除定时器、取消订阅、解绑事件等

```javascript
beforeDestroy() {
    console.log('beforeDestroy: 实例销毁前');
    console.log(this.$el);  // <div id="app">...</div>
    console.log(this.$data); //{message: "Hello, Vue! Updated!"}
    console.log(this.message); // "Hello, Vue! Updated!"
    console.log("--------------------");
},
```

## destroyed

官网：实例销毁后调用。该钩子被调用后，对应 Vue 实例的所有指令都被解绑，所有的事件监听器被移除，所有的子实例也都被销毁。

组件无法使用，`data`和`methods`也都不可使用，即使更改了实例的属性，页面的`DOM`也不会重新渲染。适合做彻底释放资源

```javascript
destroyed() {
    console.log('destroyed: 实例销毁后');
    console.log(this.$el);  // <div id="app">...</div>
    console.log(this.$data); //{message: "Hello, Vue! Updated!"}
    console.log(this.message); // "Hello, Vue! Updated!"
    console.log("--------------------");
},
```

执行完dom失去活性，无法进行任何操作。

## 示例

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vue生命周期</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <h1>{{ message }}</h1>
        <button @click="updateMsg">updateMsg</button>
        <button @click="destroyVue">destroyVue</button>
        <p>Vue实例的生命周期包括创建、挂载、更新和销毁等阶段。</p>
        <ul>
            <li>created: 实例创建完成后调用</li>
            <li>mounted: 实例挂载到DOM后调用</li>
            <li>updated: 数据更新后调用</li>
            <li>destroyed: 实例销毁前调用</li>
        </ul>
    </div>
    
</body>
<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello, Vue!'
        },
        beforeCreate() {
            console.log('beforeCreate: 实例创建前');
            console.log(this); //VueComponent {...}  当前Vue实例
            console.log(this.$el);  //undefined  当前组件实例对应的DOM元素根结点
            console.log(this.$data); //undefined 
            console.log(this.msg); // undefined
            console.log("--------------------");
        },
        created() {
            console.log('created: 实例创建完成');
            console.log(this); //VueComponent {...}  当前Vue实例
            console.log(this.$el);  //undefined 
            console.log(this.$data); //{message: "Hello, Vue!"}
            console.log(this.message); // "Hello, Vue!"
            console.log("--------------------");
        },
        beforeMount() {
            console.log('beforeMount: 实例挂载前');
            console.log(this.$el);  // <div id="app">...</div>
            console.log(this.$data); //{message: "Hello, Vue!"}
            console.log(this.message); // "Hello, Vue!"
            console.log("--------------------");
        },
        mouted() {
            console.log('mounted: 实例挂载完成');
            console.log(this.$el);  // <div id="app">...</div>
            console.log(this.$data); //{message: "Hello, Vue!"}
            console.log(this.message); // "Hello, Vue!"
            console.log("--------------------");
        },
        beforeUpdate() {
            console.log('beforeUpdate: 数据更新前');
            console.log(this.$el);  // <div id="app">...</div>
            console.log(this.$data); //{message: "Hello, Vue!"}
            console.log(this.message); // "Hello, Vue!"\
            debugger
            console.log("--------------------");
        },
        updated() {
            console.log('updated: 数据更新后');
            console.log(this.$el);  // <div id="app">...</div>
            console.log(this.$data); //{message: "Hello, Vue! Updated!"}
            console.log(this.message); // "Hello, Vue! Updated!"    
        },
        beforeDestroy() {
            console.log('beforeDestroy: 实例销毁前');
            console.log(this.$el);  // <div id="app">...</div>
            console.log(this.$data); //{message: "Hello, Vue! Updated!"}
            console.log(this.message); // "Hello, Vue! Updated!"
            console.log("--------------------");
        },
        destroyed() {
            console.log('destroyed: 实例销毁后');
            console.log(this.$el);  // <div id="app">...</div>
            console.log(this.$data); //{message: "Hello, Vue! Updated!"}
            console.log(this.message); // "Hello, Vue! Updated!"
            console.log("--------------------");
        },
        methods: {
            updateMsg() {
                this.message = 'Hello, Vue! Updated!';
            },
            destroyVue() {
                this.$destroy();
            }
        },
    });
</script>
</html>
```

## Vue3对比（选项式API）

Vue3 生命周期名字改成了 `onXxx` 钩子（组合式 API），例如：

- `beforeCreate` / `created` → **setup()**
- `beforeMount` → `onBeforeMount`
- `mounted` → `onMounted`
- `beforeUpdate` → `onBeforeUpdate`
- `updated` → `onUpdated`
- `beforeDestroy` → `onBeforeUnmount`
- `destroyed` → `onUnmounted`

## 父子组件生命周期

### 创建过程

创建过程主要涉及`beforeCreate`、`created`、`beforeMount`、`mounted`四个钩子函数。

```js
Parent beforeCreate -> Parent Created -> Parent BeforeMount -> Child BeforeCreate -> Child Created -> Child BeforeMount -> Child Mounted -> Parent Mounted
```

### 更新过程

更新过程主要涉及`beforeUpdate`、`updated`两个钩子函数，当父子组件有数据传递时才会有生命周期的比较。

```
Parent BeforeUpdate -> Child BeforeUpdate -> Child Updated -> Parent Updated
```

### 销毁过程

销毁过程主要涉及`beforeDestroy`、`destroyed`两个钩子函数，本例直接调用`vm.$destroy()`销毁整个实例以达到销毁父子组件的目的。

```
Parent BeforeDestroy -> Child BeforeDestroy -> Child Destroyed -> Parent Destroyed
```

## 参考

```shell
https://blog.csdn.net/hello_woman/article/details/127507138
https://v2.cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E9%92%A9%E5%AD%90
```

