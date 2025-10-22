# Vue2和Vue3区别对比

## 概述

- Vue2基于 **Options API**（选项式API），Vue3基于 **Composition API**（组合式API）
- 渲染引擎：Vue2基于Vue 2 Renderer，Vue3重写了虚拟DOM+编译优化，使得速度、性能更快。
- Vue2体积较大(~30KB gzipped)，Vue3更小 (~10KB gzipped)
- 响应式：Vue2基于`Object.defineProperty`，Vue3基于`Proxy`
- Vue3原生支持TS
- 生命周期命名，Vue2：`beforeCreate` / `created` ...，Vue3：改为 `setup` + `onMounted` 等函数形式
- 自定义指令
- 全局API，Vue2：`Vue.use`, `Vue.mixin`, `Vue.filter`，Vue3：`app.use`, `app.mixin`, `app.config.globalProperties`

## 响应式原理

在 Vue 开发中，我们修改了数据，所有用到这份数据的视图都会更新。响应式概括来说就是**数据驱动视图的自动更新**

### Vue2

Vue2是基于`Object.defineProperty`，通过 **“数据劫持 + 发布订阅”** 模式实现的。

- 利用 `Object.defineProperty()` 把对象的每个属性都转换为 getter/setter；
- 在 getter 收集依赖（Watcher）；
- 在 setter 通知依赖更新。

```js
function defineReactive(obj, key, val) {
  const dep = [] // 存放依赖（watcher）

  Object.defineProperty(obj, key, {
    get() {
      dep.push(window.watcher) // 收集依赖
      return val
    },
    set(newVal) {
      val = newVal
      dep.forEach(fn => fn()) // 通知更新
    }
  })
}
```

```js
const data = { count: 0 }
defineReactive(data, 'count', data.count)

// 模拟 watcher
window.watcher = () => console.log('视图更新:', data.count)
console.log(data.count) // 触发 getter
data.count = 2          // 触发 setter -> 更新
```

**缺陷**：

**无法检测新增/删除属性**

```
Vue.set(obj, 'newProp', 123) // 必须用 Vue.set 才能触发响应式
delete obj.prop              // 不会触发更新
```

**数组变化无法监听索引修改**

```
vm.arr[0] = 99   // 不会更新
vm.arr.splice(0,1,99) // 必须这样才能更新
```

**初始化时需递归遍历所有对象属性**

- 初始化性能较差；
- 大对象深层嵌套时开销很大。

### Vue3

Vue 3 完全重写响应式系统（称为 **Reactivity API**），使用 **ES6 Proxy** 实现。

- `reactive()`：创建响应式对象
- `effect()`：注册副作用函数（类似 Watcher）
- `track()` / `trigger()`：依赖收集与派发更新

```js
const targetMap = new WeakMap()

function reactive(target) {
  return new Proxy(target, {
    get(obj, key) {
      track(obj, key)       // 收集依赖
      return Reflect.get(obj, key)
    },
    set(obj, key, value) {
      Reflect.set(obj, key, value)
      trigger(obj, key)     // 触发更新
      return true
    }
  })
}

function track(obj, key) {
  let depsMap = targetMap.get(obj)
  if (!depsMap) targetMap.set(obj, (depsMap = new Map()))
  let dep = depsMap.get(key)
  if (!dep) depsMap.set(key, (dep = new Set()))
  dep.add(activeEffect)
}

function trigger(obj, key) {
  const depsMap = targetMap.get(obj)
  const dep = depsMap.get(key)
  dep && dep.forEach(effect => effect())
}

let activeEffect
function effect(fn) {
  activeEffect = fn
  fn()
  activeEffect = null
}

// 使用示例
const state = reactive({ count: 0 })
effect(() => console.log('count 改变:', state.count))
state.count++
```

**优点**：

**无需递归遍历**

- Proxy 只在访问属性时才劫持（**惰性监听**），性能更好。

**支持动态属性与嵌套对象**

```
const obj = reactive({})
obj.a = 1 // 会自动变成响应式！
```

**数组、Map、Set 完全支持**

```
const m = reactive(new Map())
m.set('a', 1) // 触发更新！
```

**性能大幅提升**

- 内存更少；
- 依赖跟踪精细化；
- 不再全量递归。







