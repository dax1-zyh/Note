---

---



# Vuex与全局事件总线

![image-20211209155310334](C:\Users\13704\AppData\Roaming\Typora\typora-user-images\image-20211209155310334.png)



上图是使用全局事件总线进行多组件数据共享，如果组件BCD需要获得组件A中的数据，或者A需要回调BCD中的数据，需要频繁地调用`eventbus`，操作复杂且繁琐。



![image-20211209155336864](C:\Users\13704\AppData\Roaming\Typora\typora-user-images\image-20211209155336864.png)



上图是通过Vuex实现多组件数据共享，将组件ABCD需要共享的数据存储在了Vuex组件中，这样相当于只需要单次进行组件间地双向通信即可共享到数据。



![image-20211209203730656](C:\Users\13704\AppData\Roaming\Typora\typora-user-images\image-20211209203730656.png)



**Vuex与事件总线的本质区别**

- `eventBus`利用事件抛发的原理进行传递数据
- `Vuex`是响应式的，和数据双向绑定的原理一样，使用了`getter`和`setter`进行数据劫持



## Vuex是什么

专门在Vue中实现集中式状态（数据）管理的一个Vue**插件**，对Vue应用中多个组件的共享状态进行集中式的管理，也是一种组件间通信的方式，且适用于任意组件间通信。Vuex 也集成到 Vue 的官方调试工具 devtools extension (opens new window)，提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。



## Vuex的优势

- 能够在vuex中集中管理共享的数据，便于开发和后期进行维护
- 能够高效的实现组件之间的数据共享，提高开发效率
- 存储在vuex中的数据是**响应式**的，当数据发生改变时，页面中的数据也会同步更新



## 使用Vuex的场景

- 多个组件依赖于同一状态
- 来自不同组建的行为需要变更同一状态



## Vuex原理图

![image-20211209160613234](C:\Users\13704\AppData\Roaming\Typora\typora-user-images\image-20211209160613234.png)

-  Actions，Mutations，State都是Object对象

- actions用于响应组件中的动作

- mutations用于操作数据（state）

- state用于存储数据

  

PS：若没有网络请求或其他业务逻辑，组件中也可以越过actions，即不写`dispatch`，直接编写`commit`



## Vuex的用法

1. 安装`Vuex`
2. 创建`Store`对象管理数据
3. 挂载`Store`对象到`Vue实例`



- 安装`Vuex`

  ```
  npm i -S vuex
  ```



- 创建`Store`对象管理数据

  ①创建`src/store`目录，并在其中创建`index.js`文件用于导入并创建`Store`对象

  ②编写`src/store/index.js`文件

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
export default new Vuex.Store({
    // state中存放的就是全局共享的数据
    state: {count: 0}
})
```

`Vue.use(Vuex)` 顺序一定要在 `new Vuex.Store` 之前



- 挂载 `Store` 对象到 `Vue` 实例

APP.vue

```js
import store from '@/store/vuex'

new Vue({
    router,
    store,
    render: (h) => h(App),
}).$mount("#app");
```



## State

提供唯一的公共数据源，所有共享的数据都要统一放到Store的state中进行存储

```js
const store = new Vuex.Store({
	state:{count:0}
})
```



### 访问State中数据的两种方式

- ```js
  this.$store.state.全局数据名称
  ```

- 导入`mapstate`函数，当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性。(可以直接用插值表达式)

  ```js
  import {mapState} from 'vuex'
  
  coumputed:{
  	...mapState(['count'])
  }
  ```



## Mutations

Mutation用于变更Store中的数据

1. 只能通过mutation变更Store数据，不可以直接操作Store中数据
2. 通过这种方式虽然操作起来稍微繁琐一点，但是可以集中监控有所数据的变化



### 触发mutations的两种方式

定义Mutation

```js
export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    add (state) {
      // 变更状态
      state.count++
    }
  },
})
```

- 触发Mutation **（第一种）**

```js
  methods: {
    handle1 () {
      // 触发mutations的第一种方式
      this.$store.commit('add')
    }
  }
```



可以在触发mutations传递参数

```js
export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    addN (state , step) {
      // 变更状态
      state.count += step
    }
  },
})
```

```js
  methods: {
    handle2 () {
      // 触发mutations的第一种方式
      this.$store.commit('addN',3)
    }
  }
```



- **第二种**

  导入 `mapMutations`函数，将需要的mutations函数，映射为当前组建的methods方法

```js
import { mapState } from 'vuex'

  methods: {
    ...mapMutations(['add', 'addN'])
  }
```



例

```js
<template>
  <div>
    <h3>现在显示的数字是 {{ this.$store.state.count }}</h3>
    <button @click="handleAdd">+1</button>
    <button @click="handleAddN">+3</button>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  name: 'add.vue',
  data () {
    return {}
  },
  methods: {
    ...mapMutations(['addN']),
    handleAdd () {
      // 触发mutations的第一种方式
      this.$store.commit('add')
    },
      // 触发mutations的第二种方式
    handleAddN () {
      this.addN(3)
    }
  }
}
</script>
```



**mutation中不能执行异步操作！！！对vue调试器有影响**



## Action

Action用于处理异步任务

如果通过异步操作变更数据，必须通过Action，而不能使用Mutation，但是在Action中还是要通过触发Mutation的方式间接变更数据。



### 触发Action的两种方式

定义Action

```js
export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    add (state) {
      // 变更状态
      state.count++
    }
  },
  actions: {
    addAsync (context) {
      setTimeout(() => {
        context.commit('add')
      }, 1000)
    }
  },
})
```



- 触发Action **（第一种）**


```js
methods:{
    handleAddAsync () {
     	this.$store.dispatch('addAsync')   
    }
}
```



触发Aciton异步任务时携带参数

```js
export default new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    addN (state , step) {
      state.count += step
    }
  },
  actions: {
    addNAsync (context) {
      setTimeout(() => {
        context.commit('addN', step)
      }, 1000)
    }
  },
})
```



触发Action

```js
methods:{
    handleAddAsync () {
     	this.$store.dispatch('addNAsync',3)   
    }
}
```



**commit触发Mutation**

**dispatch触发Action** 



- **第二种**

  导入`mapActions`函数，将需要的actions函数，映射为当前组件的methods方法

  ```js
  export default new Vuex.Store({
    state: {
      count: 0
    },
    mutations: {
      desN (state, step) {
        state.count -= step
      }
    },
    actions: {
      desNAsync (context) {
        setTimeout(() => {
          context.commit('desN', 3)
        }, 1000)
      }
    },
  })
  ```

  ```html
  <script>
  import { mapActions, mapMutations, mapState } from 'vuex'
  
  methods: {
      ...mapActions(['desNAsync']),
      handleDesNAsync () {
        this.desNAsync()
      }
    }
  </script>
  ```

  

##  Getters

`getter`是对 `Store`中已有的数据加工处理形成新的数据，类似Vue的计算属性，`Store`数据发生变化，则 `getter`中的数据也会跟着变



定义getters

```js
export default new Vuex.Store({
  state: {
    count: 0
  },
  getters: {
    showNum (state) {
      return '当前最新的数量是' + state.count
    }
  },
})
```



### 触发getters的两种方式



- ```js
  this.$store.getters.名称
  ```

- 导入`mapGetters`函数，将需要的getters函数，映射为当前组件的computed方法

  ```js
  <script>
  import { mapGetters } from 'vuex'
  
    computed: {
      ...mapGetters(['showNum'])
    }
  </script>
  ```

  