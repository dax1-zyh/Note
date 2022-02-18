## v-model的双向绑定

`v-model`是`v-bind`和`v-on`配合使用的语法糖

```javascript
<input v-model="value" />

// 就是相当于: 
<input :value="value" @input="value= $event.target.value" />
```



## v-model在自定义组件中的使用

为了让我们的组件支持`v-model`双向绑定，组件需要接受一个`props` 并发出一个`input`事件。

```html
// 父组件

<template>
  <div>
    <Dialog :visible="x" @update:visible="x = $event"></Dialog>
    <!--等价于-->
    <Dialog v-model:visible="x"></Dialog>
  </div>
</template>
```



```js
// 子组件 Dialog

<script lang="ts">
export default {
  props: {
    visible: {
      type: Boolean,
      default: false
    }
  },
  setup(props, context) {
    const close = () => {
      context.emit('update:visible', true)
    }
    return {close}
  }
};
</script>
```



在自定义组件中使用`v-model`，将`visible`的值传给子组件，子组件接收`prop(visible)`，同时，在利用`update:visivle`事件(一般使用input事件，这里是个人习惯)将值通过`$emit`,将值再传递给父组件，直接赋值给`visible`,这样就形成了自定义组件的一个双向数据绑定。