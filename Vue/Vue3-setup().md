## 组合式API

在Vue2中，一个组件通通包含`data、computed、methods、watch`等组件选项来组织逻辑，当组件变得越来越大时，**逻辑关注点**的列表也会增长，尤其对于那些一开始没有编写这些组件的人来说，这会导致组件难以阅读和理解。

这种碎片化使得理解和维护复杂组件变得困难。选项的分离掩盖了潜在的逻辑问题。此外，在处理单个逻辑关注点时，我们必须不断地“跳转”相关代码的选项块。

如果能够将同一个逻辑关注点相关代码收集在一起会更好。而这正是组合式 API 使我们能够做到的。

因此，Vue3引入了 `setup` 组件选项，用于实现组合式API。



## setup在生命周期的位置

`setup`位于`beforeCreate`和`created`之前，用于替代两者,在组件创建之前执行，一旦`props`被解析，就将作为组合式API的入口

此外，可以通过在生命周期钩子前面加上 “on” 来访问组件的生命周期钩子。

下表包含如何在`setup()`内部调用生命周期钩子：

|    选项式 API     | Hook inside `setup` |
| :---------------: | :-----------------: |
|  `beforeCreate`   |     Not needed*     |
|     `created`     |     Not needed*     |
|   `beforeMount`   |   `onBeforeMount`   |
|     `mounted`     |     `onMounted`     |
|  `beforeUpdate`   |  `onBeforeUpdate`   |
|     `updated`     |     `onUpdated`     |
|  `beforeUnmount`  |  `onBeforeUnmount`  |
|    `unmounted`    |    `onUnmounted`    |
|  `errorCaptured`  |  `onErrorCaptured`  |
|  `renderTracked`  |  `onRenderTracked`  |
| `renderTriggered` | `onRenderTriggered` |
|    `activated`    |    `onActivated`    |
|   `deactivated`   |   `onDeactivated`   |

```js
export default {
  setup() {
    // mounted
    onMounted(() => {
      console.log('Component is mounted!')
    })
  }
}
```



## this

**在`setup()`内部，`this`指向的不是vue实例**，因为`setup()`是在解析其他组件选项之前被调用的。所以 `setup()` 内部的 `this` 的行为与其它选项中的 `this` 完全不同。这使得 `setup()` 在和其它选项式 API 一起使用时可能会导致混淆。



## setup参数

`setup()`接收两个参数

- `props`
- `context`



## props

`setup` 函数中的第一个参数是 `props`。`props` 是响应式的，当传入新的 prop 时，它将被更新。

```js
export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title)
  }
}
```

但是，因为 props 是响应式的，你不能使用 ES6 解构，它会消除 prop 的响应性。

如果需要解构 prop，可以在 `setup` 函数中使用 [`toRefs`](https://v3.cn.vuejs.org/guide/reactivity-fundamentals.html#响应式状态解构) 函数来完成此操作：

```js
import { toRefs } from 'vue'

setup(props) {
  const { title } = toRefs(props)

  console.log(title.value)
}
```

如果 `title` 是可选的 prop，则传入的 `props` 中可能没有 `title` 。在这种情况下，`toRefs` 将不会为 `title` 创建一个 ref 。你需要使用 `toRef` 替代它：

```js
import { toRef } from 'vue'
setup(props) {
  const title = toRef(props, 'title')
  console.log(title.value)
}
```



## context

传递给 `setup` 函数的第二个参数是 `context`。`context` 是一个普通 JavaScript 对象，暴露了其它可能在 `setup` 中有用的值：

```js
export default {
  setup(props, context) {
    // Attribute (非响应式对象，等同于 $attrs)
    console.log(context.attrs)

    // 插槽 (非响应式对象，等同于 $slots)
    console.log(context.slots)

    // 触发事件 (方法，等同于 $emit)
    console.log(context.emit)

    // 暴露公共 property (函数)
    console.log(context.expose)
  }
}
```

`context` 是一个普通的 JavaScript 对象，也就是说，它不是响应式的，这意味着你可以安全地对 `context` 使用 ES6 解构。

```js
export default {
  setup(props, { attrs, slots, emit, expose }) {
    ...
  }
}	
```



## 访问组件的property

执行 `setup` 时，你只能访问以下 property：

- `props`
- `attrs`
- `slots`
- `emit`

换句话说，你**将无法访问**以下组件选项：

- `data`
- `computed`
- `methods`
- `refs` (模板 ref)\



## 结合模板使用

**`setup()`必须返回一个对象**，一旦return，就可以像vue2的方式那样使用该属性

```vue
<template>
  <div>{{ collectionName }}: {{ readersNumber }} {{ book.title }}</div>
</template>

<script>
  import { ref, reactive } from 'vue'

  export default {
    props: {
      collectionName: String
    },
    setup(props) {
      const readersNumber = ref(0)
      const book = reactive({ title: 'Vue 3 Guide' })

      // 暴露给 template
      return {
        readersNumber,
        book
      }
    }
  }
</script>
```



## 组件选项返回优先级

如果`data,props,setup`都有一个同名属性,`setup`返回的该属性优先级最高.



## 如何调用子组件setup内的方法

1. 子组件在`setup`写好方法`method`,并通过`return`暴露出去

2. 父组件调用子组件时为其添加`ref`属性

3. 父组件`setup`内拿到(2)添加的`ref`属性`property`,再通过`property.value.method()`调用子组件

   

## 参考文档

[介绍 | Vue.js (vuejs.org)](https://v3.cn.vuejs.org/guide/composition-api-introduction.html)

[Setup | Vue.js (vuejs.org)](https://v3.cn.vuejs.org/guide/composition-api-setup.html#参数)

[生命周期钩子 | Vue.js (vuejs.org)](https://v3.cn.vuejs.org/guide/composition-api-lifecycle-hooks.html)

[vue3之setup的使用理解 - 简书 (jianshu.com)](https://www.jianshu.com/p/f47556e726de)