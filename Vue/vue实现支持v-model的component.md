> 一个组件上的`v-model`默认会利用名为`value`的 `prop`和名为`input`的事件，但是像单选框、复选框等类型的输入控件可能会将`valueattribute`用于不同的目的。`model`选项可以用来避免这样的冲突


```javascript
Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```

在自定义组件中定义`model`选项，并指定传输值`prop`和回调事件`event`