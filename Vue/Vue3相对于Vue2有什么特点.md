**Vue3特点**

- 创建实例的方式不同，`createApp()`代替了`new Vue()`
- 组合式API`composition API`
- `data`始终声明为函数
- `template`模板下可以有不止一个元素
- 生命周期变化，`destroyed`改名为`unmounted`
- 数据响应式用`Proxy`实现，Vue2是`define.property`
- 拥抱`TypeScript`