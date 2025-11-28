# Vuex使用(非模块化)

## 1.引入vuex

**store/index.js**

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    count: 0,
    user: null
  },
  getters: {
    doubleCount: state => state.count * 2
  },
  mutations: {
    setCount(state, value) {
      state.count = value
    },
    setUser(state, user) {
      state.user = user
    }
  },
  actions: {
    async fetchUser({ commit }) {
      const user = await getUserFromAPI()
      commit('setUser', user)
    }
  }
})
```

**main.js**

```js
import Vue from 'vue'
import App from './App.vue'
import store from './store'

new Vue({
  store,
  render: h => h(App)
}).$mount('#app')
```

## 2.组件中使用

### 直接读取

```vue
<script>
export default {
  computed: {
    count() {
      return this.$store.state.count;
    },
    user() {
      return this.$store.state.user;
    },
    double() {
      return this.$store.getters.double;
    }
  },
  methods: {
    increment() {
      this.$store.commit("increment", payload);
    },
    incrementAsync() {
      this.$store.dispatch("incrementAsync", payload);
    }
  }
};
</script>
```

### 使用map辅助函数

```vue
<template>
  <div>
    <p>count: {{ count }}</p>
    <p>double: {{ double }}</p>

    <button @click="increment">增加</button>
    <button @click="incrementAsync">异步增加</button>
  </div>
</template>

<script>
import { mapState, mapGetters, mapMutations, mapActions } from "vuex";

export default {
  computed: {
    ...mapState(["count", "user"]),
    ...mapGetters(["double"]),
  },
  methods: {
    ...mapMutations(["increment"]),
    ...mapActions(["incrementAsync"]),
  }
};
</script>
```

### 如何传参

```js
getters: {
  getUserById: (state) => (id) => {
    return state.users.find(u => u.id === id);
  }
},
actions: {
  incrementBy({ commit }, payload) {
    commit('increment', payload);
  }
},
mutations: {
    increment(state, payload) {
      state.count = payload
    }
},    
```

其中不使用mapGetters，在组件中使用`this.$store.getters["getUserById"](1);  `

使用mapGetters，在组件中使用`this.getUserById(1);  `

# Vuex使用（模块化）

## 1.引入Vuex

**store/modules/user.js**

```js
export default {
  namespaced: true,
  state: () => ({
    info: null
  }),
  mutations: {
    setInfo(state, data) {
      state.info = data
    }
  },
  actions: {
    async load({ commit }) {
      const res = await fetch('/api/user')
      commit('setInfo', res)
    }
  },
  getters: {
    isLogin: state => !!state.info
  }
}
```

**store/index.js**

```js
import Vue from 'vue'
import Vuex from 'vuex'
import user from './modules/user'

Vue.use(Vuex)

export default new Vuex.Store({
  modules: {
    user
  }
})
```

**main.js**

```js
import Vue from 'vue'
import App from './App.vue'
import store from './store'

new Vue({
  store,
  render: h => h(App)
}).$mount('#app')
```

## 2.组件中使用

示例

```js
// store/modules/user.js
export default {
  namespaced: true,

  state: {
    name: "Tom",
    age: 20
  },

  getters: {
    info(state) {
      return `${state.name} - ${state.age}`;
    },

    // 带参数 getter
    greet: (state) => (msg) => {
      return `${msg}, ${state.name}`;
    }
  },

  mutations: {
    setName(state, newName) {
      state.name = newName;
    }
  },

  actions: {
    async updateName({ commit }, newName) {
      commit("setName", newName);
    }
  }
};
```

### 直接读取

```vue
<script>
export default {
  computed: {
    userName() {
      return this.$store.state.user.name; // state
    },
    userInfo() {
      return this.$store.getters["user/info"]; // getter
    }
  },
  methods: {
    changeName() {
      this.$store.commit("user/setName", "Jerry"); // mutation
    },

    updateNameAsync() {
      this.$store.dispatch("user/updateName", "Alice"); // action
    },

    // getter 传参
    greetMsg() {
      return this.$store.getters["user/greet"]("Hi");  
    }
  }
}
</script>
```

### 使用map辅助函数

```vue
<template>
  <div>
    <p>{{ name }} - {{ age }}</p>
	<p>{{ info }}</p>
    <button @click="increment">增加</button>
    <button @click="incrementAsync">异步增加</button>
  </div>
</template>

<script>
import { mapState, mapGetters, mapMutations, mapActions } from "vuex";

export default {
  computed: {
    ...mapState("user", ["name", "age"]),
    ...mapGetters("user", ["info"])
  },
  methods: {
    ...mapMutations("user", ["setName"]),
    ...mapActions("user", ["updateName"]),
      
    // getter 传参
    greetMsg() {
      return this.info("Hi");  
    },
    changeName() {
      this.setName("Jack"); //mutations
    },
    updateNameAsync() {
      this.updateName("Peter"); // action
    },
  }
};
</script>
```

### 多模块使用map避免重名

对不同模块相同变量或方法起别名

```vue
computed: {
  ...mapState('user', { userName: 'name' }),
  ...mapState('product', { productName: 'name' }),

  ...mapGetters('user', { userInfo: 'info' }),
  ...mapGetters('product', { productInfo: 'info' }),
},

methods: {
  ...mapMutations('user', { setUserName: 'setName' }),
  ...mapMutations('product', { setProductName: 'setName' }),

  ...mapActions('user', { updateUserName: 'updateName' }),
  ...mapActions('product', { updateProductName: 'updateName' }),
}
```
