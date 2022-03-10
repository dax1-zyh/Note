## Vue2.x
Vue2是通过`new Vue()`，构造函数的方式来创建实例

```javascript
import Vue from 'Vue'
import APP from './APP.vue'

const vm = new Vue({
	render:h => h(APP)
})
vm.$mount('#app')
```

## Vue3.x
Vue3通过`createApp`工厂函数来创建实例

```javascript
import {createApp} from 'vue'
import APP from './APP.vue'

const app = createApp(APP)
app.mount('#app')
```

## 区别
### Vue2
Vue2的组件系统设计中，所有Vue实例是共享一个Vue构造函数对象的，包括全局指令/全局组件，无法做到相互隔离。

也就是说我们整个项目中，只有一个**根Vue实例**，其他的单文件组件创建的 Vue实例都会成为它的**子实例**。


```javascript
<body>
    <div id="app1"></div>
    <div id="app2"></div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
    <script>
        Vue.component('hello-world',{
            template:`<div>hello world</div>`
        })
        const app1=new Vue({
            template:`<hello-world></hello-world>`
        })
        app1.$mount('#app1')
        
        const app1=new Vue({
            template:`<hello-world></hello-world>`
        })
        app1.$mount('#app2')
    </script>
</body>
```

如上代码中，创建了两个实例app1和app2，两个实例中都可以使用全局组件hello-world。

### Vue3
而Vue3通过createApp方法可以返回一个提供应用上下文的应用实例。不同实例注册的组件无法在不同的实例下使用。
![在这里插入图片描述](https://img-blog.csdnimg.cn/4e441fffd00948bfaf74d92ce007a86a.png)例如，通过 app1.component 注册组件，无法在 app2 中使用
![在这里插入图片描述](https://img-blog.csdnimg.cn/0ca9b256204349dfa56215e8c24284c9.png)

## 使用场景
由上可得，Vue3更适合在大型的开发环境中使用，不同开发人员可以完全独立地使用不同的实例。