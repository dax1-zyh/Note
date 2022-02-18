## 属性默认传递给根元素

在组件间传递时，父组件传入给子组件的属性，会默认传递给子组件的最外层标签上。

例如，在 `ButtonDemo`组件中导入了 `Button`组件，并将`onClick`属性传递给子组件

```html
// ButtonDemo.vue

<template>
  <div>
    <Button @click="onClick" @focus="onFoces" size="normal">按钮</Button>
  </div>
</template>

<script lang="ts">
import Button from '../lib/Button.vue'
export default {
  components: {Button},
  setup() {
    const onClick = () => {
      console.log('hi')
    }
    return {onClick}
  }
}
</script>
```

```html
// Button.vue

<template>
   <button>
     <slot/>
   </button>
</template>
```

在`Button`组件中未定义任何事件，但是在使用这个组件的时候传入的事件却可以使用，就是因为vue会把你传给这个组件上的所有事件，默认传给最外层的元素。



## 内层元素继承属性

如果不想让子组件最外层的元素绑定属性，则需要使用`inheritAttrs:false`，这样就可以让内层的元素继承属性。

```html
// Button.vue

<template>
  <div>
    <button>
      <slot></slot>
    </button>
  </div>
</template>
<script lang="ts">
export default {
  inheritAttrs: false,
};
</script>
```



## 指定元素继承属性

如果想要让div里面的button标签绑定上这些属性，可以用`v-bind="$attrs"`

```html
<template>
  <div>
    <button v-bind="$attrs">
      <slot/>
    </button>
  </div>
</template>
```



## 分开继承属性

如果想要一部分绑定给button标签，一部分绑定给div标签，我们可以借助`context.attrs`

```html
<template>
  <div :size="size">
    <button v-bind="rest">
      <slot></slot>
    </button>
  </div>
</template>
<script lang="ts">
export default {
  inheritAttrs: false,
  setup(props, context) {
    const { size, ...rest } = context.attrs;
    return { size, rest };
  },
};
</script>
```

此处，将size属性给div标签继承，剩余的属性给button标签继承。



## 总结

- 默认所有属性都绑定到根元素
- 使用 `inheritAttrs:false`可以取消默认绑定
- 使用`$attrs`或者`context.attrs`获取所有属性
- 使用`v-bind="$attrs"`批量绑定属性
- 使用 `const {size, ...rest} = context.attrs`将属性分开继承



## v-bind和v-model的区别

- v-bind 是标签内**属性的单向绑定**
- v-model 是标签外**值的双向绑定**



## props和attrs的区别

- props需要先声明才能获取值，而attrs则不用
- props声明过的属性，attrs里面不会在出现
- props不包含事件，attrs包含
- props支持string以外的类型，而attrs只有string类型